# Step 6: System Architecture & Data Flow Design

## 1. High-Level System Architecture

TruthGuard implements a federated micro-frontend topology combined with serverless Edge Functions and client-side forensic engines. The system unifies decentralized forensic tools under an NVIDIA-inspired dark glassmorphism dashboard.

```mermaid
flowchart TB
    subgraph ClientLayer ["Client Browser Layer (Chrome / Firefox / Edge)"]
        HubPortal["Master Hub Portal (Port 5173 /)"]
        VeritDashboard["VeritAI Executive Dashboard (/dashboard)"]
        PortProber["Dynamic Port Prober (/features/:id)"]
        
        subgraph SubFrontends ["Federated Micro-Frontends"]
            F1_UI["TruthLence UI (Port 5176)"]
            F2_UI["DEEP SCAN Camera UI (Port 5183)"]
            F3_UI["DeepGuard Telecom UI (Port 5182)"]
            F4_UI["MediaVault Drive UI (Port 5184)"]
        end

        subgraph ClientEngines ["In-Browser Forensic Computation"]
            WebCrypto["Web Crypto API: SHA-256 Digesting"]
            ExifrParser["exifr Binary Metadata & GPS Parser"]
            ZustandStore["Zustand In-Memory & LocalStorage Store"]
        end
    end

    subgraph EdgeLayer ["Serverless Cloud Infrastructure (Supabase Edge / Deno)"]
        EdgeMedia["analyze-media Edge Function: Deepfake & Fact-Check"]
        EdgeContent["analyze-content Edge Function: Vishing & Smishing NLP"]
    end

    subgraph DataLayer ["Cloud Persistence & External Grounding"]
        AuthDB[("Supabase GoTrue: auth.users")]
        PostgresDB[("Supabase PostgreSQL: profiles table")]
        FactCheckFeeds["Grounding Feeds: PolitiFact, Snopes, FactCheck.org, Reuters"]
    end

    HubPortal --> SubFrontends
    PortProber ==>|HEAD no-cors| SubFrontends
    VeritDashboard <-->|JWT / RLS| PostgresDB
    
    F2_UI --> WebCrypto
    F2_UI --> ExifrParser
    F4_UI --> ZustandStore

    F1_UI -->|HTTPS POST| EdgeMedia
    F3_UI -->|HTTPS POST| EdgeContent

    EdgeMedia --> FactCheckFeeds
    VeritDashboard --> FactCheckFeeds
```

---

## 2. Request Lifecycles (Top 3 Features)

### 1. Camera Tamper & EXIF Forensics Lifecycle (`F2`)
```mermaid
sequenceDiagram
    autonumber
    actor Analyst as Forensic Analyst
    participant UI as camara-temper/Index.tsx
    participant Analyzer as lib/fileAnalyzer.ts
    participant Crypto as window.crypto.subtle
    participant ExifLib as exifr Parser
    participant Gauge as components/AuthenticityGauge.tsx

    Analyst->>UI: Drops CCTV_frame_04.jpg
    UI->>UI: setIsAnalyzing(true)
    UI->>Analyzer: analyzeFile(selectedFile)
    Analyzer->>Crypto: digest('SHA-256', arrayBuffer)
    Crypto-->>Analyzer: 64-char Hex Digest
    Analyzer->>ExifLib: parse(file, true)
    ExifLib-->>Analyzer: EXIF, GPS, and Software Tags
    Analyzer->>Analyzer: Audit tags: Deduct -20 if Photoshop/GIMP detected
    Analyzer-->>UI: Return FileMetadata (Score: 45%)
    UI->>Gauge: Animate SVG radial gauge to 45% (Amber)
    UI->>Analyst: Display "Danger: Editing Software Detected (Photoshop)"
```

### 2. Multi-Modal Deepfake & Fact-Checking Lifecycle (`F1`)
```mermaid
sequenceDiagram
    autonumber
    actor Journo as Investigative Journalist
    participant View as truthlence/Index.tsx
    participant Client as integrations/supabase/client.ts
    participant Edge as Supabase Edge: analyze-media
    participant UI as components/AnalysisResult.tsx

    Journo->>View: Enters news URL / drops article screenshot
    View->>View: Truncate text payload to 5,000 characters
    View->>Client: functions.invoke("analyze-media", { body })
    Client->>Edge: POST /functions/v1/analyze-media
    Edge->>Edge: Analyze generative diffusion artifacts
    Edge->>Edge: Cross-reference claims with fact-check feeds
    Edge-->>Client: 200 OK { verdict: "fake", confidence: 92, details: [...] }
    Client-->>View: Resolve parsed data
    View->>UI: Mount AnalysisResult with Framer Motion slide-up
    UI-->>Journo: Render 92% fake probability + cited PolitiFact links
```

### 3. Scam Call Transcript Triage Lifecycle (`F3`)
```mermaid
sequenceDiagram
    autonumber
    actor Operator as SOC Incident Responder
    participant Page as calldetection/CallDetection.tsx
    participant Supa as Supabase Client
    participant Edge as Edge Function: analyze-content
    participant Card as components/ResultCard.tsx

    Operator->>Page: Pastes executive impersonation transcript
    Page->>Supa: invoke("analyze-content", { type: "call", content })
    Supa->>Edge: POST /functions/v1/analyze-content
    Edge->>Edge: Extract linguistic markers: urgency, financial wire request
    Edge-->>Supa: 200 OK { verdict: "fake", confidence: 95, indicators: [...] }
    Supa-->>Page: Return JSON payload
    Page->>Card: Mount ResultCard with red border & danger bar
    Card-->>Operator: Displays FAKE DETECTED (95% Confidence)
```

---

## 3. Database Schema & Data Modeling

TruthGuard leverages PostgreSQL on Supabase Cloud, governed by Row Level Security (RLS) to enforce strict role-based access.

```mermaid
erDiagram
    PROFILES {
        uuid id PK "Primary Key (Default gen_random_uuid())"
        uuid user_id FK "References auth.users(id) ON DELETE CASCADE"
        text display_name "Analyst full name"
        text avatar_url "Profile image URI"
        text role "Journalist | Analyst | Admin"
        text provider "email | google"
        timestamp created_at "Default now()"
        timestamp updated_at "Default now()"
    }

    AUTH_USERS {
        uuid id PK "GoTrue user identifier"
        string email "Unique registered email"
        string encrypted_password "Bcrypt / Argon2 hash"
        timestamp last_sign_in_at "Session timestamp"
    }

    PROFILES ||--|| AUTH_USERS : "belongs_to"
```

### Database Indexes & Optimization
* **Index on `profiles.user_id`**:
  - `CREATE UNIQUE INDEX idx_profiles_user_id ON public.profiles(user_id);`
  - *Reasoning*: Accelerates point lookups during authentication profile retrieval in [`AuthContext.tsx`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/mainpage/src/contexts/AuthContext.tsx#L41) to under 1ms.

---

## 4. Authentication & Authorization Flow

```mermaid
sequenceDiagram
    autonumber
    actor User as Security Officer
    participant UI as VeritAIApp.tsx / AuthModal
    participant Context as AuthContext.tsx
    participant GoTrue as Supabase Auth (GoTrue)
    participant PG as PostgreSQL (profiles table)

    User->>UI: Enters email and password
    UI->>Context: login(email, password)
    Context->>GoTrue: signInWithPassword({ email, password })
    GoTrue-->>Context: Returns Session JWT & User UUID
    Context->>PG: SELECT * FROM profiles WHERE user_id = user.id
    PG-->>Context: { display_name, role: "Analyst" }
    Context->>Context: setUser(Profile) & setSession(Session)
    Context-->>UI: Re-render with Analyst permissions & avatar initials
```

---

## 5. AI Integration & Inference Pipeline

* **Prompt Construction**: Modality-specific envelopes encapsulate raw inputs with strict safety guardrails.
* **Structured Output Parsing**: The edge function formats outputs into typed JSON schemas (`AnalysisData` and `TelecomResult`), guaranteeing that client components receive predictable numerical scores and array indicators.
* **Cost & Token Control**: Text submissions are capped at 5,000 characters ([`truthlence/src/pages/Index.tsx:L73`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/truthlence/src/pages/Index.tsx#L73)) to prevent token exhaustion and control inference costs.
* **Fallback Strategy**: If an AI provider experiences downtime, the edge function falls back to deterministic heuristic regex pattern matching, ensuring the client UI never crashes.

---

## 6. Scaling to 100x Traffic & The First Three Bottlenecks

```mermaid
flowchart TD
    Current[Current Architecture: Direct Client-to-Edge Calls] --> Scale100x[Scaling to 100x Traffic Volume]
    
    Scale100x --> B1["Bottleneck 1: Edge Function Rate Limits & API Quotas"]
    Scale100x --> B2["Bottleneck 2: Browser LocalStorage Quota Exceeded (5MB Limit)"]
    Scale100x --> B3["Bottleneck 3: Synchronous Dynamic Port Prober Network Congestion"]

    B1 --> Sol1["Solution 1: Redis Semantic Caching & BullMQ Async Job Queues"]
    B2 --> Sol2["Solution 2: Client IndexedDB & S3 Object Lock Cloud Storage"]
    B3 --> Sol3["Solution 3: NGINX Reverse Proxy with Centralized Service Discovery"]
```

1. **Bottleneck 1: Edge Function Concurrency & AI Provider Quotas**:
   - *Issue*: 100x concurrent users uploading images will trigger rate-limit throttling (HTTP 429) from inference APIs.
   - *Mitigation*: Introduce an asynchronous queue (BullMQ or AWS SQS) to level load peaks, combined with a Redis semantic cache that returns identical cached verdicts for duplicate SHA-256 hashes.
2. **Bottleneck 2: Browser LocalStorage 5MB Capacity**:
   - *Issue*: MediaVault's `localStorage` persistence crashes with `QuotaExceededError` if hundreds of Base64 files are stored locally.
   - *Mitigation*: Transition client storage to IndexedDB via `idb-keyval` (supporting gigabytes of offline storage) and offload primary asset storage to AWS S3 or Supabase Storage.
3. **Bottleneck 3: Port Prober Socket Flooding**:
   - *Issue*: Running HEAD requests across 24 local ports per user causes socket contention.
   - *Mitigation*: In production, replace multi-port local development with a single unified NGINX reverse proxy routing path-based micro-frontends (`/truthlence`, `/camera`, `/vault`).
