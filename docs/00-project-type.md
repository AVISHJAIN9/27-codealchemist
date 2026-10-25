# Step 0 Pre-Check: Project Type Detection & Evidence Audit

## 1. Detected Project Type
* **Primary Classification**: Multi-Modal Cyber-Defense & AI Forensics Platform (React 18 + TypeScript + Vite + Tailwind CSS + Supabase Edge Functions + Micro-Frontend Federated Hub Architecture).
* **Stack Paradigm**: Distributed Frontend-Centric Architecture with Serverless Edge Functions (`supabase.functions.invoke`), Client-Side Cryptographic & Forensic Analysis Engines (Web Crypto API SHA-256, EXIF parsing via `exifr`), and State-Driven Real-Time Observability (Zustand, TanStack Query, Framer Motion).
* **Applicable Documentation Track**:
  - **Track B (Frontend-Centric Forensics & Micro-Frontends)**: Advanced Component Design, Dynamic Port Probing (`mainpage/src/pages/FeatureEmbed.tsx`), Canvas/SVG Authenticity Gauges, Client-Side Forensic Parsers, Zustand Persistent Stores.
  - **Track A/C Hybrid (Serverless Backend & Edge Gateways)**: Supabase PostgreSQL `profiles` schema, Edge Function invocations (`analyze-media`, `analyze-content`), OAuth2 / JWT authentication flows (`mainpage/src/contexts/AuthContext.tsx`).
  - **AI Integration Track**: Pre-trained multi-modal forensic heuristics, sentiment & emotional manipulation analysis, fact-checking consensus pipelines (PolitiFact, Snopes, FactCheck.org, Reuters integrations), and voice/SMS phishing threat classifiers.

---

## 2. Evidence from Codebase

| Evidence Item | Source Location in Workspace | Architectural Role & Justification |
|---|---|---|
| **Hub Orchestration** | [`mainpage/src/pages/Hub.tsx`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/mainpage/src/pages/Hub.tsx#L1-L217) | Central access point coordinating 6 specialized forensic sub-modules with NVIDIA-inspired dark styling. |
| **Dynamic Port Prober** | [`mainpage/src/pages/FeatureEmbed.tsx`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/mainpage/src/pages/FeatureEmbed.tsx#L59-L76) | Implements client-side dynamic port scanning (`checkPort` starting at port 5176 using `no-cors` HEAD requests) to detect running micro-frontend instances. |
| **Executive Forensics** | [`mainpage/src/components/VeritAIApp.tsx`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/mainpage/src/components/VeritAIApp.tsx#L1-L1685) | 1,685-line comprehensive forensic dashboard calculating credibility scores (0–100), emotion manipulation indexes, bias indicators, and claim verification. |
| **Cryptographic Provenance** | [`mediavault 2/src/lib/mediavault-store.ts`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/mediavault%202/src/lib/mediavault-store.ts#L64-L120) | Zustand store with local persistence (`STORAGE_KEY_FILES`), simulating multi-stage encryption, file integrity verification, and activity logging. |
| **Tamper Analysis & EXIF** | [`camara-temper/src/lib/fileAnalyzer.ts`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/camara-temper/src/lib/fileAnalyzer.ts#L66-L179) | Forensic file analyzer combining Web Crypto SHA-256 (`crypto.subtle.digest`) with `exifr` parsing to detect editing software signatures (`photoshop`, `gimp`, `lightroom`). |
| **Edge Function Invocations** | [`truthlence/src/pages/Index.tsx`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/truthlence/src/pages/Index.tsx#L35-L38) & [`calldetection-sms-contentdetection/src/pages/CallDetection.tsx`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/calldetection-sms-contentdetection/src/pages/CallDetection.tsx#L23-L26) | Remote serverless AI invocations via `supabase.functions.invoke('analyze-media')` and `supabase.functions.invoke('analyze-content')`. |
| **Authentication & RBAC** | [`mainpage/src/contexts/AuthContext.tsx`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/mainpage/src/contexts/AuthContext.tsx#L68-L99) | Supabase Auth listener (`onAuthStateChange`) mapped to relational `profiles` table with role definitions (`Journalist`, `Analyst`, `Admin`). |

---

## 3. How the Documentation Adapts

1. **Micro-Frontend Architecture Focus**: Rather than documenting a traditional single-monolith Express backend, we document the real-world micro-frontend federated topology: independent Vite applications communicating over designated local ports (`:5173`, `:5182`, `:5183`, `:5184`) and unified via the Hub and Edge gateways.
2. **AI Integration Integrity**: Real code calls pre-trained AI analysis routines via Supabase Edge Functions (`analyze-media` and `analyze-content`) and client-side heuristics. No custom machine learning weights were trained locally; models are accessed as inference services and deterministic algorithmic pipelines.
3. **Database Grounding**: Database features reflect the real PostgreSQL / Supabase schema (the `profiles` table and session storage), while asset metadata caching uses client-side IndexedDB and `localStorage` persistence in `mediavault 2`.
4. **Mandatory Artifacts**: All diagrams (Mermaid), first-person spoken scripts ("Say It Out Loud"), the 275-item enterprise master checklist, the 19 system design deep dives, and the 150+ project-specific interview Q&As reflect this exact implementation.
