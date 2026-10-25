# Step 1: Project Map & Structural Audit

## 1. Complete Codebase Folder Tree

```text
33-codealchemist/ (TruthGuard Workspace Root)
├── ARCHITECTURE.md                  # System architecture specification and data flow diagrams
├── HUB_SETUP_GUIDE.md               # Multi-port federation and setup guide for the micro-frontend hub
├── QUICK_START.md                   # 30-second bootstrap instructions for local multi-service development
├── README.md                        # Master project overview, features summary, and module portfolio
├── package.json                     # Root documentation workspace dependencies (VitePress, Mermaid)
├── docs/                            # Vue.js-style documentation suite (Interview-ready engineering guide)
│   ├── .vitepress/                  # VitePress theme, dark styling (pure black #000000), and build config
│   ├── features/                    # Dedicated deep dives for forensic modules F1 through F5
│   └── system-design/               # Comprehensive enterprise system design deep dive (19 chapters)
│
├── mainpage/                        # Master Orchestration Hub & VeritAI Executive Dashboard
│   ├── vite.config.ts               # Vite build pipeline and path aliases (@/* -> src/*)
│   └── src/
│       ├── App.tsx                  # Main router configuration (/, /dashboard, /features/:id)
│       ├── main.tsx                 # React 18 DOM mount point (createRoot)
│       ├── contexts/
│       │   └── AuthContext.tsx      # Supabase Auth provider, session listener, and profiles table sync
│       ├── components/
│       │   ├── VeritAIApp.tsx       # 1,685-line executive dashboard: credibility scores, MBFC tiers, claim audit
│       │   ├── Navigation.tsx       # Cross-project header with return-to-hub routing
│       │   └── ui/                  # Radix UI primitive components (toast, sonner, tooltip, buttons)
│       ├── pages/
│       │   ├── Hub.tsx              # Master landing portal with 6 animated cards and NVIDIA dark theme
│       │   ├── Index.tsx            # Dashboard page host rendering VeritAIApp
│       │   ├── FeatureEmbed.tsx     # Dynamic port prober scanning localhost:5176–5200 with no-cors HEAD
│       │   └── NotFound.tsx         # 404 error page with quick-navigation back to Hub
│       └── integrations/
│           ├── supabase/client.ts   # Supabase client instantiation with persistent localStorage
│           └── lovable/             # Lovable OAuth provider integration
│
├── truthlence/                      # Multi-Modal Fact-Checking & Deepfake Analysis Subsystem
│   ├── vite.config.ts               # Subsystem bundler configured for port 5176
│   └── src/
│       ├── App.tsx                  # Application router and query provider
│       ├── pages/
│       │   └── Index.tsx            # Tabbed interface: file upload, text analysis, and URL fact-checker
│       ├── components/
│       │   ├── FileUploadZone.tsx   # Drag-and-drop zone with MIME filtering
│       │   ├── AnalysisResult.tsx   # Visual verdict card (authentic/suspicious/fake) with confidence gauge
│       │   ├── AnalysisHistory.tsx  # Interactive list of past forensic analysis records
│       │   ├── AnalyzingOverlay.tsx # Radar scanning animation during Edge Function invocation
│       │   ├── StatsBar.tsx         # Real-time counter of authentic vs suspicious vs fake detections
│       │   ├── TextAnalysisInput.tsx# Raw prose editor with 5,000-character truncation threshold
│       │   └── UrlAnalysisInput.tsx # Web URL submission for automated DOM extraction
│       └── integrations/supabase/   # Supabase client and database schema definitions
│
├── camara-temper/                   # DEEP SCAN Optical Tampering & Camera Forensics Subsystem
│   ├── vite.config.ts               # Subsystem bundler configured for port 5174 / 5183
│   └── src/
│       ├── pages/
│       │   └── Index.tsx            # Forensic file analyzer host: hero, upload, gauge, and metadata grid
│       ├── lib/
│       │   └── fileAnalyzer.ts      # Core engine: Web Crypto SHA-256 + exifr EXIF parser + tamper heuristics
│       └── components/
│           ├── AuthenticityGauge.tsx# Radial SVG gauge rendering clamped 0–100 authenticity score
│           ├── MetadataPanel.tsx    # Hardware audit: Make, Model, Lens, ISO, Shutter, and editing tags
│           ├── GpsMap.tsx           # GPS latitude/longitude visualizer
│           ├── FilePreview.tsx      # Media raster renderer with dimension diagnostics
│           └── FileUploadZone.tsx   # High-contrast file drop component
│
├── calldetection-sms-contentdetection/ # DeepGuard Telecommunications Fraud Subsystem
│   ├── vite.config.ts               # Subsystem bundler configured for port 5173 / 5182
│   └── src/
│       ├── App.tsx                  # Multi-route navigator for call, SMS, and content analyzers
│       ├── pages/
│       │   ├── Index.tsx            # DeepGuard portal with cybersecurity stats and feature links
│       │   ├── CallDetection.tsx    # Vishing and deepfake voice transcript analyzer
│       │   ├── SmsDetection.tsx     # Smishing and fraudulent SMS text classifier
│       │   └── ContentDetection.tsx # Synthesized AI vs human text discriminator
│       └── components/
│           ├── Navbar.tsx           # Subsystem navigation bar with active route glow
│           └── ResultCard.tsx       # Threat alert card with confidence bar and key indicators
│
└── mediavault 2/                    # MEDIAVAULT DRIVE Cryptographic Storage Subsystem
    ├── vite.config.ts               # Subsystem bundler configured for port 5175 / 5184
    └── src/
        ├── pages/
        │   └── Index.tsx            # Ingest pipeline host: ticker, hero, storage widget, dropzone, and queue
        ├── lib/
        │   ├── mediavault-store.ts  # Zustand store with localStorage persistence and multi-stage upload
        │   └── mediavault-types.ts  # Type schemas for QueueItem, DriveFile, and ActivityEntry
        └── components/mediavault/
            ├── Ticker.tsx           # Live running forensic metrics ticker
            ├── TopNav.tsx           # Section switcher (upload, library, activity, settings)
            ├── Hero.tsx             # Cyberpunk hero banner with ingest statistics
            ├── StorageWidget.tsx    # Visual disk usage bar and file type distribution breakdown
            ├── DropZone.tsx         # Drag-and-drop ingest area
            ├── URLInput.tsx         # Remote URL submission for direct vault download
            ├── QueuePanel.tsx       # Staged upload queue with progress and status controls
            ├── FileLibrary.tsx      # Filterable asset library grid with search and deletion
            ├── ActivityLog.tsx      # Chronological audit trail of all custody actions
            ├── ProgressModal.tsx    # Multi-stage upload simulation (Reading -> Encrypting -> Storing)
            ├── FileDetailModal.tsx  # Detailed forensic inspector displaying SHA-256, type, and size
            └── SettingsPanel.tsx    # Google Drive folder sync and data purge configuration
```

---

## 2. Technology Stack & Exact Versions

### Workspace Root Dependencies ([`package.json`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/package.json#L10-L14))
* `vitepress` (`^1.6.3`): Static documentation generator powering the Vue.js-style engineering guide.
* `mermaid` (`^11.4.1`): Architecture and sequence diagram rendering engine.
* `vitepress-plugin-mermaid` (`^2.0.17`): Seamless VitePress integration for markdown-native Mermaid diagrams.

### Subsystem Dependencies (Detected across `mainpage`, `truthlence`, `camara-temper`, etc.)

| Dependency Name | Version Scope | Technical Role & Necessity |
|---|---|---|
| `react` & `react-dom` | `^18.3.1` | Core UI library providing declarative rendering, concurrent hooks, and reconciliation. |
| `typescript` | `^5.5.3` | Static type safety ensuring strict interface contracts across complex forensic structures. |
| `vite` | `^5.4.1` | Next-generation frontend bundler providing instant HMR and optimized Rollup builds. |
| `@supabase/supabase-js` | `^2.45.4` | Client SDK managing authentication, PostgREST API queries, and Edge Function dispatch. |
| `exifr` | `^7.1.3` | High-speed binary parser extracting EXIF, IPTC, XMP, and GPS tags from image headers. |
| `zustand` | `^5.0.0` | Minimalist reactive state manager with local storage persistence used in `mediavault 2`. |
| `@tanstack/react-query` | `^5.56.2` | Async server state manager managing cache invalidation and query lifecycles. |
| `framer-motion` | `^11.0.8` | Declarative animation engine powering glassmorphism cards and radar sweeps. |
| `lucide-react` | `^0.446.0` | Accessible SVG icon suite for cybersecurity and forensic status indicators. |
| `tailwindcss` | `^3.4.11` | Utility-first styling framework enabling custom NVIDIA dark gradients and glow filters. |
| `clsx` & `tailwind-merge` | `^2.1.1` & `^2.5.2` | Dynamic class string concatenation preventing conflicting Tailwind rule overrides. |
| `@radix-ui/react-*` | `^1.1.0` | Accessible headless primitives for dialogs, tooltips, and toast notifications. |

---

## 3. Environment Variables & Secrets Reference

| Environment Variable | Issuing Authority | Purpose | Consumption Site in Code | How to Obtain |
|---|---|---|---|---|
| `VITE_SUPABASE_URL` | Supabase Cloud | Root URL endpoint for the Supabase backend cluster. | `src/integrations/supabase/client.ts` in all modules | Supabase Dashboard -> Project Settings -> API -> Project URL |
| `VITE_SUPABASE_PUBLISHABLE_KEY` | Supabase Cloud | Public anonymous API key with Row Level Security (RLS) enforcement. | `src/integrations/supabase/client.ts` in all modules | Supabase Dashboard -> Project Settings -> API -> `anon` public key |

*Security Rule: Never commit `.env` or `.env.local` files containing production credentials to version control. Public publishable keys only grant permissions allowed by Supabase RLS policies.*

---

## 4. Third-Party Services & External APIs

1. **Supabase Cloud (GoTrue & PostgreSQL)**:
   - Provides user authentication (`signInWithPassword`, `signUp`, `signOut`).
   - Serves relational data queries for the `profiles` table.
2. **Supabase Edge Functions (Deno Runtime)**:
   - `analyze-media`: Multi-modal generative artifact detection and fact-check aggregation.
   - `analyze-content`: NLP classifier for phone transcripts and SMS text.
3. **Lovable OAuth Provider**:
   - Federated identity gateway facilitating Google OAuth sign-in.
4. **Media Bias/Fact Check (MBFC) & Fact-Checking Syndications**:
   - Reference datasets embedded into `VeritAIApp.tsx` (PolitiFact, Snopes, FactCheck.org, Reuters) to validate extracted factual claims.

---

## 5. Application Entry Points

* **Root Documentation Entry**: `package.json` -> `npm run docs:dev` (`vitepress dev docs`) launches the VitePress documentation server on `http://localhost:5173`.
* **Main Hub Entry**: `mainpage/src/main.tsx` initializes React DOM, mounts `App.tsx`, and boots on port 5173.
* **TruthLence Entry**: `truthlence/src/main.tsx` boots the deepfake inspection module on port 5176.
* **Camera Temper Entry**: `camara-temper/src/main.tsx` boots the DEEP SCAN optical analyzer on port 5183.
* **Call & SMS Detection Entry**: `calldetection-sms-contentdetection/src/main.tsx` boots the DeepGuard telecommunications classifier on port 5182.
* **MediaVault Entry**: `mediavault 2/src/main.tsx` boots the cryptographic drive on port 5184.
