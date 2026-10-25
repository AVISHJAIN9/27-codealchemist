# F2: DEEP SCAN Camera Tamper & Lens Obstruction Engine

## 1. Feature Identity & Behavioral Overview
* **Feature Name**: DEEP SCAN Optical Camera Tamper & Forensic Metadata Analyzer
* **Identifier**: `F2`
* **One-Line Description**: Inspects raw surveillance frames and photographic evidence for physical camera lens tampering, software modification signatures, and stripped metadata.
* **User-Facing Behavior**: When accessing the DEEP SCAN module (`http://localhost:5183`), the user drops an image, video, audio, or data file into [`FileUploadZone.tsx`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/camara-temper/src/components/FileUploadZone.tsx). The system transitions to an analyzing state, parses file bytes, and renders a forensic command dashboard featuring an SVG [`AuthenticityGauge.tsx`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/camara-temper/src/components/AuthenticityGauge.tsx) (0–100 score), a detailed [`MetadataPanel.tsx`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/camara-temper/src/components/MetadataPanel.tsx) listing camera hardware specs (Make, Model, Lens, Focal Length, Aperture, Shutter Speed, ISO, Flash, Color Space), a cryptographic SHA-256 fingerprint, and an embedded [`GpsMap.tsx`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/camara-temper/src/components/GpsMap.tsx) if coordinates exist.

---

## 2. Architecture & Flow Diagrams

### Component Interaction Flowchart
```mermaid
flowchart TD
    User([Forensic Investigator]) --> Upload[FileUploadZone.tsx]
    Upload --> HandleSelect[handleFileSelect in Index.tsx:L17]
    HandleSelect --> Delay[UX Stabilization Delay 1500ms]
    Delay --> FileAnalyzer[analyzeFile in fileAnalyzer.ts:L66]
    
    subgraph AnalysisEngine ["fileAnalyzer.ts Pipeline"]
        ComputeHash[computeHash: Web Crypto SHA-256 Digest]
        ImageDims[getImageDimensions: Natural Width & Height]
        ExifParser[exifr.parse: Parse EXIF, TIFF, GPS]
        HeuristicCheck[Heuristic Authenticity Algorithm]
    end

    FileAnalyzer --> ComputeHash
    FileAnalyzer --> ImageDims
    FileAnalyzer --> ExifParser
    
    ComputeHash --> HeuristicCheck
    ImageDims --> HeuristicCheck
    ExifParser --> HeuristicCheck

    HeuristicCheck --> MetaState[setMetadata in Index.tsx:L26]
    
    MetaState --> Gauge[AuthenticityGauge.tsx: Radial SVG]
    MetaState --> MetaGrid[MetadataPanel.tsx: Camera Specs & Flags]
    MetaState --> Preview[FilePreview.tsx: Media Canvas]
    MetaState --> GPS[GpsMap.tsx: Coordinate Mapping]
```

### Request & Processing Sequence Diagram
```mermaid
sequenceDiagram
    autonumber
    actor Investigator as SOC Investigator
    participant View as camara-temper/Index.tsx
    participant Analyzer as lib/fileAnalyzer.ts
    participant Crypto as window.crypto.subtle
    participant ExifLib as exifr Parser
    participant UI as components/MetadataPanel.tsx

    Investigator->>View: Drops CCTV_frame_04.jpg
    View->>View: setIsAnalyzing(true) & setFile(selectedFile)
    View->>Analyzer: analyzeFile(selectedFile)
    
    Analyzer->>Crypto: digest('SHA-256', arrayBuffer)
    Crypto-->>Analyzer: 256-bit raw cryptographic hash
    Analyzer->>Analyzer: Format hex string (fileHash)
    
    Analyzer->>ExifLib: parse(file, true)
    ExifLib-->>Analyzer: { Make, Model, Lens, ISO, FNumber, DateTimeOriginal, Software, latitude, longitude }
    
    Analyzer->>Analyzer: Audit Software tags against ['photoshop', 'gimp', 'canva', 'figma']
    alt Tampering Detected
        Analyzer->>Analyzer: Push Danger Flag + Dock 20 Points
    else Authentic Hardware Identified
        Analyzer->>Analyzer: Push Success Flag + Add 10 Points
    end
    
    Analyzer-->>View: Returns FileMetadata object
    View->>View: setMetadata(result) & setIsAnalyzing(false)
    View->>UI: Render hardware specs, SHA-256 hash & danger warnings
```

### Forensic Data Entity Relationship
```mermaid
erDiagram
    FILE_METADATA {
        string fileName "CCTV_frame_04.jpg"
        number fileSize "Bytes"
        string fileType "JPG / PNG / MP4"
        string mimeType "image/jpeg"
        string fileHash "SHA-256 64-char Hexadecimal Digest"
        number authenticityScore "Clamped 0 to 100"
        number width "Natural Width px"
        number height "Natural Height px"
        string cameraMake "e.g., Sony / Hikvision"
        string cameraModel "e.g., Alpha 7 / DS-2CD2042"
        string software "Detected Software Tag"
        number latitude "GPS Coordinate"
        number longitude "GPS Coordinate"
    }

    ANALYSIS_FLAG {
        string type "info | warning | danger | success"
        string message "Descriptive Flag Title"
        string detail "Detailed Technical Finding"
    }

    FILE_METADATA ||--o{ ANALYSIS_FLAG : "contains"
```

### Failure & Graceful Degradation Diagram
```mermaid
flowchart TD
    FileDropped[File Dropped into Zone] --> ValidateMime{Inspect MIME Type}
    
    ValidateMime -->|Image File| ProcessImage[Extract Dimensions & EXIF]
    ValidateMime -->|Video / Audio| ProcessMedia[Log Limited Metadata Warning]
    ValidateMime -->|Generic Document| ProcessDoc[Assign Baseline Score 50]
    
    ProcessImage --> ExifTry{Try exifr.parse}
    ExifTry -->|Corrupted EXIF / Stripped Header| CatchExif[Empty Catch: Graceful Fallback]
    ExifTry -->|Valid EXIF| ScoreHeuristics[Calculate Hardware Score]
    CatchExif --> ScoreHeuristics
    
    ScoreHeuristics --> ClampScore[Math.max 0, Math.min 100]
    ProcessMedia --> ClampScore
    ProcessDoc --> ClampScore
    
    ClampScore --> RenderView[Display Diagnostics Panel]
```

---

## 3. Function & Implementation Inventory

| Function Name | Location | Parameters | Return Type | Purpose & Implementation Details |
|---|---|---|---|---|
| `analyzeFile` | [`camara-temper/src/lib/fileAnalyzer.ts:L66-179`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/camara-temper/src/lib/fileAnalyzer.ts#L66-L179) | `file: File` | `Promise<FileMetadata>` | Main forensic orchestrator. Computes SHA-256 digest, extracts image dimensions, parses EXIF/GPS, audits software manipulation tags, and computes 0–100 authenticity score. |
| `computeHash` | [`camara-temper/src/lib/fileAnalyzer.ts:L50-55`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/camara-temper/src/lib/fileAnalyzer.ts#L50-L55) | `file: File` | `Promise<string>` | Invokes browser Web Crypto API `crypto.subtle.digest('SHA-256', buffer)` and maps resulting bytes to hexadecimal notation. |
| `getImageDimensions` | [`camara-temper/src/lib/fileAnalyzer.ts:L57-64`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/camara-temper/src/lib/fileAnalyzer.ts#L57-L64) | `file: File` | `Promise<{width, height}>` | Instantiates a native `new Image()`, creates an object URL, and resolves `naturalWidth` and `naturalHeight` upon load. |
| `handleFileSelect` | [`camara-temper/src/pages/Index.tsx:L17-28`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/camara-temper/src/pages/Index.tsx#L17-L28) | `selectedFile: File` | `Promise<void>` | Sets loading state, executes a 1,500ms stabilization delay for visual telemetry, awaits `analyzeFile`, and sets metadata state. |
| `handleReset` | [`camara-temper/src/pages/Index.tsx:L30-34`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/camara-temper/src/pages/Index.tsx#L30-L34) | None | `void` | Resets file, metadata, and loading states back to initial idle view. |

---

## 4. Engineering Quality, Edge Cases & Security

* **Heuristic Scoring Model**:
  - Baseline score: `75`.
  - Camera make & model confirmed: `+10` points (`success` flag).
  - Missing camera data: `-10` points (`warning` flag - possible synthetic generation).
  - Original capture date present: `+5` points; missing: `-5` points.
  - GPS coordinates present: `+5` points (`info` flag).
  - Software tampering detected (`photoshop`, `gimp`, `lightroom`, `snapseed`, `canva`, `figma`): `-20` points (`danger` flag).
  - Clamping function: `Math.max(0, Math.min(100, authenticityScore))` ensures invariant bounded outputs.
* **Edge Cases & Graceful Degradation**:
  - Non-image files (videos, audio, PDFs) do not crash the binary parser. Isolated `try-catch` blocks absorb parse failures and assign informative fallback flags (`"Limited metadata extraction"`).
* **Security & Memory Safety**:
  - All hash computations execute within the secure browser sandbox via `window.crypto.subtle`.
  - Object URLs created during dimension checking are transiently handled to avoid DOM memory leaks.

---

## 5. Say It Out Loud (Spoken Interview Script)

> "In DEEP SCAN, located in `camara-temper/src/lib/fileAnalyzer.ts`, I built our client-side camera tamper and forensic metadata analyzer. In video surveillance and digital evidence, attackers frequently tamper with cameras using physical occlusions like spray paint, or manipulate digital frames in editors like Photoshop before submission.
> 
> When an investigator drops a suspect media file, `handleFileSelect` initiates analysis. First, I use the browser's native Web Crypto API to calculate a cryptographic SHA-256 hash directly from the file buffer, creating an immutable tamper-evident fingerprint. Next, I parse EXIF and TIFF headers using `exifr` to extract low-level hardware attributes like camera make, lens model, aperture, ISO, and GPS coordinates.
> 
> My heuristic algorithm starts at a baseline authenticity score of 75. If camera make and model match genuine hardware, I add 10 points. If original capture dates are stripped, I dock 5 points. Crucially, I inspect the software tags against known raster editors like Photoshop, GIMP, Canva, and Figma. If an editing signature is detected, I dock 20 points and raise a high-severity danger alert.
> 
> I chose native client-side Web Crypto and `exifr` rather than uploading images to a remote server. This allows analysis to complete in milliseconds, preserves evidence chain of custody, and guarantees that sensitive surveillance footage never leaves the investigator's machine.
> 
> If a file has corrupted EXIF headers, the parsing routine catches the error silently, falls back to raw binary metrics, and flags that metadata was stripped.
> 
> One current limitation is that video optical flow analysis is limited in the browser. If I rebuilt this for an enterprise SOC, I would integrate WebGL canvas shaders to analyze frame-by-frame histogram deltas in real time."
