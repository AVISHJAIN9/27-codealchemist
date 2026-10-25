# Step 10: Master Interview Preparation (150+ Questions & In-Depth Answers)

This guide provides 150 project-specific technical and behavioral questions with full answers tailored for college internship and fresher full-stack/frontend security engineering roles.

---

## 1. High-Priority Opening Questions

### Q1: "Walk me through your project in 3 minutes."
**Answer:**
"TruthGuard is a multi-modal cyber-defense and digital media forensics platform built to combat generative AI deepfakes, optical camera tampering, and voice phishing scams. With the rise of diffusion models and voice cloning, traditional security filters fail to detect synthetic media. I designed TruthGuard as a federated micro-frontend suite connecting five specialized engines under an NVIDIA-inspired dark glassmorphism dashboard.

The first engine is TruthLence, which inspects uploaded imagery, text, and URLs for generative compression artifacts and cross-references assertions against fact-checking databases like PolitiFact and Snopes via Supabase Edge Functions. The second is DEEP SCAN, an optical camera tamper monitor. It runs purely in the browser using the Web Crypto API to calculate SHA-256 hashes and `exifr` to extract EXIF headers, identifying camera hardware specs, GPS coordinates, and flagging editing software tags like Photoshop or Canva. The third engine is DeepGuard, which analyzes call transcripts and SMS messages for voice synthesis patterns, social engineering urgency, and financial wire fraud. The fourth is MediaVault, a cryptographic asset custody drive built with Zustand that handles file ingestion, multi-stage encryption progress, and immutable activity logging. And fifth is the VeritAI Executive Dashboard, a 1,600-line analytical command center with radial confidence gauges, source reliability tiers, and sensationalism scoring.

Architecturally, I built a dynamic port prober in `FeatureEmbed.tsx` that scans localhost ports from 5176 to 5200 with non-blocking HEAD requests to discover running micro-frontend instances dynamically. By pairing hardware-accelerated client-side cryptography with serverless edge AI, TruthGuard provides sub-second forensic verification while keeping sensitive media completely private on the analyst's machine."

### Q2: "Give me your 60-second elevator pitch."
**Answer:**
"I built TruthGuard to solve the synthetic media crisis in cybersecurity. Today, bad actors use generative voice cloning and deepfakes to bypass biometric gates, spoof executives on phone calls, and tamper with surveillance feeds. TruthGuard brings together five specialized forensic tools into a single workstation: image deepfake detection, physical camera tamper auditing, telecommunication scam triage, cryptographic evidence custody, and an executive credibility dashboard.

Rather than bundling heavy machine learning weights into the client, I combined client-side Web Crypto SHA-256 hashing and binary EXIF parsing with serverless Supabase Edge Functions. This keeps our client bundle under 300 kilobytes while delivering sub-second threat confidence scores. It gives security analysts and journalists transparent, explainable forensic verification they can trust."

### Q3: "What was the single hardest function you wrote in this codebase? Explain it line by line."
**Answer:**
"The hardest and most critical function is `analyzeFile` located in [`camara-temper/src/lib/fileAnalyzer.ts:L66-179`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/camara-temper/src/lib/fileAnalyzer.ts#L66-L179). It orchestrates cryptographic binary hashing, asynchronous DOM image dimension extraction, binary EXIF parsing, and heuristic tamper scoring.

Here is how it works line by line:
1. **Lines 67–68**: Initializes an empty `flags` array and sets a baseline `authenticityScore` of 75.
2. **Line 70**: Calls `await computeHash(file)`. Inside `computeHash` (lines 50–55), it reads `file.arrayBuffer()`, passes the raw bytes into `crypto.subtle.digest('SHA-256', buffer)`, maps the resulting 32 bytes into unsigned 8-bit integers, and formats them as a 64-character hexadecimal string.
3. **Lines 72–82**: Initializes the `FileMetadata` object with name, size, extension, last modified date, MIME type, baseline score, and the calculated SHA-256 hash.
4. **Lines 87–93**: If the file is an image, it awaits `getImageDimensions(file)`, which instantiates an in-memory `new Image()`, assigns an object URL, and resolves `naturalWidth` and `naturalHeight`.
5. **Lines 94–127**: Calls `await exifr.parse(file, true)`. `exifr` reads only the initial file chunks containing TIFF/EXIF tags. It extracts camera make, model, lens, focal length, aperture, shutter speed, ISO, flash status, software tags, and GPS coordinates (latitude, longitude, altitude).
6. **Lines 129–137**: Evaluates camera hardware tags. If make and model are identified, it pushes a `success` flag and adds 10 points (`authenticityScore += 10`). If absent, it pushes a `warning` flag and docks 10 points.
7. **Lines 138–144**: Checks `dateOriginal`. If missing, it docks 5 points because stripped timestamps often indicate online re-compression or generative synthesis.
8. **Lines 153–162**: Audits the `software` tag against an array of known raster editors: `['photoshop', 'gimp', 'lightroom', 'snapseed', 'canva', 'figma']`. If any match is found, it raises a `danger` flag and docks 20 points (`authenticityScore -= 20`).
9. **Line 175**: Executes `Math.max(0, Math.min(100, authenticityScore))` to guarantee the final score remains strictly bounded between 0 and 100 before returning."

### Q4: "What were the toughest bugs or challenges you faced, and how did you solve them?"
**Answer:**
"One of the toughest challenges was building the dynamic port prober in [`FeatureEmbed.tsx`](file:///Users/avishjain/avishjain9-repos/33-codealchemist/mainpage/src/pages/FeatureEmbed.tsx). Because each micro-frontend runs on an independent Vite port (such as 5173, 5182, 5183), hardcoding URLs broke whenever a port was in use and Vite auto-incremented to 5184 or 5185. Standard fetch calls across ports failed immediately due to browser CORS preflight blocks. I solved this by using `fetch(url, { method: 'HEAD', mode: 'no-cors' })`. Even though the response body is opaque, an active socket connection resolves the fetch promise without CORS errors, while an unallocated port triggers a connection refused error. This allowed us to recursively scan ports 5176 through 5200 with a 100ms throttle to discover running services automatically.

A second challenge was preventing `QuotaExceededError` crashes in MediaVault. When users dropped multiple images into the ingest queue, encoding them as Base64 strings quickly exceeded the browser's 5MB `localStorage` limit. I created an exception-safe wrapper `saveToStorage` that wraps storage calls in a `try-catch` block, allowing the Zustand store to gracefully fall back to in-memory state while logging a warning instead of crashing the interface."

### Q5: "If you had to rebuild this platform from scratch, what would you change?"
**Answer:**
"First, I would replace the multi-port local development setup with an NGINX reverse proxy or API gateway. Running five separate terminal windows for different micro-frontends works well for rapid prototyping, but in production, routing through a single domain with path prefixes (like `/truthlence`, `/camera`, `/vault`) simplifies SSL certificates, avoids CORS completely, and enables unified cookie propagation.

Second, I would migrate MediaVault from `localStorage` to `IndexedDB` using a library like `idb-keyval` or integrate directly with an S3-compatible object store. LocalStorage has a strict 5MB quota and blocks the main thread during JSON serialization, whereas IndexedDB supports gigabytes of binary data asynchronously.

Third, I would introduce a Web Worker for file hashing. While the Web Crypto API is fast, reading a 500-megabyte video file into an `ArrayBuffer` on the main thread consumes significant heap memory. Offloading file chunking to a background Web Worker would ensure 60 FPS UI responsiveness even during heavy file uploads."

---

## 2. Architecture & Micro-Frontend Design (Q6 – Q25)

* **Q6: Why did you choose a micro-frontend architecture over a single monolithic React app?**
  *Answer*: A monolithic security app forces all forensic models, visualizers, and heavy dependencies into a single bundle. Micro-frontends allow each tool (DEEP SCAN, DeepGuard, MediaVault) to have isolated dependencies, independent build cycles, and failure isolation—if one module crashes, the rest of the security suite remains operational.
* **Q7: How do your micro-frontends communicate if they run on different ports?**
  *Answer*: They communicate loosely via URL parameters, browser storage (`localStorage`), and shared backend APIs (Supabase). For discovery, the Hub uses client-side port probing via `FeatureEmbed.tsx`.
* **Q8: What is the purpose of `FeatureEmbed.tsx`?**
  *Answer*: It acts as a dynamic service discovery client. It tests ports from 5176 to 5200 using non-blocking HEAD requests to detect active instances of local micro-frontends and render direct launch links.
* **Q9: Why did you use `mode: 'no-cors'` in the port prober?**
  *Answer*: Browsers block cross-port requests by default if the target server does not send CORS headers. Using `no-cors` allows the socket handshake to complete and resolve the fetch promise without reading the response body.
* **Q10: What happens if an analyst accesses a feature whose port is closed?**
  *Answer*: The prober scans through port 5200, times out gracefully, and renders an informative fallback screen with instructions to run `npm run dev` in that module's directory.
* **Q11: How do you handle routing in `mainpage/src/App.tsx`?**
  *Answer*: Using `react-router-dom` v6 with `<BrowserRouter>`, defining routes for `/` (Hub), `/dashboard` (VeritAIApp), `/features/:id` (FeatureEmbed), and `*` (NotFound).
* **Q12: Why is the Hub separated from the VeritAI Dashboard?**
  *Answer*: The Hub is a broad, high-level entry portal for selecting specific forensic tools, whereas the Dashboard is an intensive executive workstation with deep analytical metrics, claim verification, and sentiment scores.
* **Q13: How is state shared between the Hub and sub-projects?**
  *Answer*: Through Supabase authentication tokens stored in browser `localStorage`, allowing an analyst logged in at the Hub to remain authenticated across tools.
* **Q14: What is the role of `Navigation.tsx`?**
  *Answer*: It provides a standardized cross-project header component with brand styling and a back-to-hub link.
* **Q15: How does the system achieve failure isolation?**
  *Answer*: Because each micro-frontend runs in its own process and viewport, an unhandled runtime error in one tool cannot corrupt the JavaScript execution context of another.
* **Q16: What build tool is used across all micro-frontends?**
  *Answer*: Vite 5, providing instant HMR and Rollup production bundling.
* **Q17: Why not use Webpack 5 Module Federation?**
  *Answer*: Module Federation introduces tight coupling between shared dependency versions. Port federation allowed each subproject to remain a completely independent, standalone repository.
* **Q18: How do you prevent port conflicts during local development?**
  *Answer*: Each module's `vite.config.ts` specifies a designated default port (e.g., 5173 for Hub, 5182 for DeepGuard, 5183 for Camera, 5184 for MediaVault), and our dynamic prober detects any automatic fallback increments.
* **Q19: What is the advantage of keeping dependencies isolated in each folder?**
  *Answer*: TruthLence can update its fact-checking libraries without forcing a rebuild or risking breakage in DEEP SCAN's EXIF parsing engine.
* **Q20: How would you deploy this architecture to production?**
  *Answer*: Deploy each frontend as an independent static site on Vercel, Netlify, or AWS S3/CloudFront behind a unified NGINX reverse proxy with path-based routing.
* **Q21: What is the role of `vite-env.d.ts` in each subproject?**
  *Answer*: It provides TypeScript type definitions for Vite client features, such as `import.meta.env` and static asset imports.
* **Q22: How is the NVIDIA dark theme enforced across subprojects?**
  *Answer*: Through a shared design system specified in Tailwind CSS configuration files with unified color tokens (`#000000`, `#0a0a0a`, `#76b900`, `#00a8e0`, `#e53935`).
* **Q23: How do you handle 404 routes?**
  *Answer*: Via `NotFound.tsx` in each subproject, rendering an error badge and a direct return button back to the Hub homepage.
* **Q24: What is the purpose of `@tanstack/react-query` in `App.tsx`?**
  *Answer*: It provides an async query client cache to manage background data refetching and request deduplication.
* **Q25: What is the main trade-off of a micro-frontend architecture?**
  *Answer*: Duplicate runtime memory when multiple modules are open simultaneously, which we mitigated by keeping each sub-bundle lightweight.

---

## 3. Cryptography & Forensic File Analysis (Q26 – Q45)

* **Q26: How does DEEP SCAN compute SHA-256 hashes without external libraries?**
  *Answer*: By calling `window.crypto.subtle.digest('SHA-256', buffer)` using the browser's native Web Crypto API.
* **Q27: Why is native Web Crypto better than CryptoJS?**
  *Answer*: It executes in compiled C++ directly on the CPU with hardware acceleration, making it up to 15 times faster while adding 0 kilobytes to our bundle.
* **Q28: How does `fileAnalyzer.ts` convert the raw hash buffer to a hex string?**
  *Answer*: `Array.from(new Uint8Array(hashBuffer)).map(b => b.toString(16).padStart(2, '0')).join('')`.
* **Q29: What is the role of `exifr` in DEEP SCAN?**
  *Answer*: It parses binary headers to extract EXIF, TIFF, XMP, IPTC, and GPS tags from image files.
* **Q30: Why is `exifr` faster than `exif-js`?**
  *Answer*: `exifr` reads only the initial file chunks containing metadata headers rather than buffering the entire multi-megabyte image.
* **Q31: What hardware metadata does DEEP SCAN extract?**
  *Answer*: Camera Make, Camera Model, Lens Model, Focal Length, Aperture (F-Number), Shutter Speed (Exposure Time), ISO, Flash status, White Balance, and Color Space.
* **Q32: How does the heuristic authenticity algorithm work?**
  *Answer*: Starts at 75. Adds +10 if camera make/model are confirmed, docks -10 if absent. Adds +5 if original capture date is present; docks -5 if missing. If editing software is detected, it docks -20.
* **Q33: What software tags trigger a tampering alert?**
  *Answer*: `photoshop`, `gimp`, `lightroom`, `snapseed`, `canva`, and `figma`.
* **Q34: How is the authenticity score clamped?**
  *Answer*: Using `Math.max(0, Math.min(100, authenticityScore))` to guarantee outputs remain between 0% and 100%.
* **Q35: How are GPS coordinates handled?**
  *Answer*: Extracted via `exif.latitude`, `exif.longitude`, and `exif.GPSAltitude`, and rendered interactively on `GpsMap.tsx`.
* **Q36: What happens if an uploaded image has no EXIF data?**
  *Answer*: The parser pushes a warning flag (`'No camera info found'`), docks 10 points, and falls back to inspecting raw file properties.
* **Q37: How are natural image dimensions extracted?**
  *Answer*: `getImageDimensions` instantiates a `new Image()`, sets `src = URL.createObjectURL(file)`, and resolves `naturalWidth` and `naturalHeight` on the `onload` event.
* **Q38: Why is client-side hashing important for evidence chain of custody?**
  *Answer*: It guarantees that evidence files never leave the analyst's workstation, preventing data leaks and compliance violations.
* **Q39: How does `camara-temper` handle video and audio files?**
  *Answer*: It identifies the MIME type and pushes an informative flag noting that deep temporal analysis requires server-side processing.
* **Q40: How does `AuthenticityGauge.tsx` render the score?**
  *Answer*: Using an SVG radial progress circle with color transitions: green (`#76b900`) for >=70, amber (`#f5a623`) for >=40, and red (`#e53935`) for <40.
* **Q41: What causes an image's capture date to differ from its modified date?**
  *Answer*: Re-saving an image in an editor updates `ModifyDate` while leaving `DateTimeOriginal` intact, providing clear proof of post-processing.
* **Q42: Can an attacker spoof EXIF metadata?**
  *Answer*: Yes, metadata can be edited using tools like ExifTool. That is why TruthGuard combines metadata audits with cryptographic hashing and AI visual artifact checks.
* **Q43: How does the system handle corrupted image files?**
  *Answer*: The parsing calls are wrapped in `try-catch` blocks that absorb binary decode errors and assign safe fallback metadata.
* **Q44: Why does TruthGuard display shutter speed as a fraction (e.g., 1/250s)?**
  *Answer*: To match standard photographic conventions, calculated as `1/${Math.round(1 / exif.ExposureTime)}s` when exposure time is less than 1 second.
* **Q45: What is the time complexity of the file analyzer?**
  *Answer*: Hashing is O(N) with file byte size; EXIF parsing is O(1) because it reads a fixed header slice.

---

## 4. Telecommunications & Scam Detection (Q46 – Q65)

* **Q46: What types of telecommunication fraud does DeepGuard detect?**
  *Answer*: Deepfake voice cloning scripts (vishing), fraudulent SMS phishing (smishing), and synthetic AI-generated text.
* **Q47: How does `CallDetection.tsx` submit transcripts?**
  *Answer*: Calls `supabase.functions.invoke("analyze-content", { body: { type: "call", content: transcript } })`.
* **Q48: What validation occurs before dispatching an SMS for analysis?**
  *Answer*: Verifies `sms.trim()` is non-empty; if empty, pops a destructive toast notification and halts execution.
* **Q49: What indicators are returned for voice scam transcripts?**
  *Answer*: Urgent wire transfer demands, credential solicitations (SSN/PIN), artificial legal threats, and synthetic cadence patterns.
* **Q50: How does `ResultCard.tsx` display threat verdicts?**
  *Answer*: Maps verdicts (`fake`, `legitimate`, `suspicious`) to distinct icons (`ShieldAlert`, `ShieldCheck`, `ShieldQuestion`) and theme colors.
* **Q51: How is the confidence bar animated in `ResultCard.tsx`?**
  *Answer*: Using Framer Motion animating width from 0% to `result.confidence%` over 1 second with `easeOut`.
* **Q52: Why are transcripts submitted via POST body instead of query parameters?**
  *Answer*: To avoid character length limits and prevent sensitive dialogue from being recorded in server access logs.
* **Q53: How does `ContentDetection.tsx` distinguish human from AI text?**
  *Answer*: It evaluates perplexity, burstiness, repetitive syntax distributions, and formulaic transitional phrasing.
* **Q54: What happens if an API call to `analyze-content` fails?**
  *Answer*: Caught in `catch (e: any)`, displaying the error message in a destructive toast while resetting the button loader.
* **Q55: What icons does DeepGuard use for visual feedback?**
  *Answer*: `ShieldCheck` for legitimate, `ShieldAlert` for fake, `ShieldQuestion` for suspicious, and `Phone`/`MessageSquare`/`FileText` for modalities.
* **Q56: How does the navbar in `calldetection` highlight the active route?**
  *Answer*: Matches `location.pathname === item.to` and applies `bg-primary/10 text-primary nv-border-glow`.
* **Q57: What stats are showcased on the DeepGuard home screen?**
  *Answer*: 99.7% Detection Accuracy, <2s Analysis Time, 50M+ Threats Detected, 24/7 Protection.
* **Q58: What is "vishing"?**
  *Answer*: Voice phishing, where attackers use phone calls—often augmented with AI voice cloning—to trick victims into sending funds or sharing credentials.
* **Q59: What is "smishing"?**
  *Answer*: SMS phishing, where fraudulent text messages containing urgent alerts lure victims into clicking credential-harvesting links.
* **Q60: How does TruthGuard handle high volumes of spam text?**
  *Answer*: Inputs are processed through serverless Deno edge workers that scale automatically with incoming request volume.
* **Q61: How could real-time audio be integrated into `CallDetection.tsx`?**
  *Answer*: Using the Web Audio API with a `MediaStreamTrack` processor and a WebAssembly Whisper speech-to-text pipeline.
* **Q62: Why is the confidence score rounded in `ResultCard.tsx`?**
  *Answer*: Displaying `Math.round(result.confidence)` provides clean, executive-friendly telemetry without distracting decimal noise.
* **Q63: What role does Framer Motion play in DeepGuard?**
  *Answer*: Provides smooth stagger entrances for cards and fluid progress animations when results load.
* **Q64: How does DeepGuard handle ambiguous content?**
  *Answer*: Classifies it as `suspicious` (amber warning) rather than forcing an inaccurate binary safe/fake decision.
* **Q65: What security precautions are taken when rendering analysis text?**
  *Answer*: Text is rendered via React JSX expressions which automatically escape dangerous HTML entities, preventing cross-site scripting (XSS).

---

## 5. Media Custody & Storage Management (Q66 – Q85)

* **Q66: What is the core responsibility of MediaVault?**
  *Answer*: Managing the secure ingestion, simulated multi-stage encryption, and immutable audit custody of digital evidence.
* **Q67: How does `useMediaVaultStore` persist state?**
  *Answer*: It serializes state to browser `localStorage` under keys `mediavault_files`, `mediavault_activity`, and `mediavault_drive_link`.
* **Q68: What are the three stages of simulated upload in `ProgressModal.tsx`?**
  *Answer*: Reading file buffer (30%), Cryptographic encryption (65%), and Committing to drive storage (100%).
* **Q69: How are files converted to Base64 in MediaVault?**
  *Answer*: `fileToBase64` wraps a native `FileReader` in a Promise, calling `reader.readAsDataURL(file)`.
* **Q70: How does MediaVault prevent `QuotaExceededError` crashes?**
  *Answer*: The `saveToStorage` helper wraps `localStorage.setItem` in a `try-catch` block, safely absorbing quota exceptions.
* **Q71: What data is recorded in `ActivityLog.tsx`?**
  *Answer*: An array of `ActivityEntry` records tracking action type (`upload`, `delete`, `download`), file name, timestamp, and asset type.
* **Q72: What metadata does `FileDetailModal.tsx` inspect?**
  *Answer*: Unique file ID, file name, MIME category, byte size, addition date, source URL, and simulated SHA-256 fingerprint.
* **Q73: How does `StorageWidget.tsx` calculate disk usage?**
  *Answer*: It aggregates the `size` property of all `driveFiles`, calculates percentage against a 10GB virtual quota, and displays category breakdowns.
* **Q74: How are file extensions categorized in `mediavault-types.ts`?**
  *Answer*: `getExtType` maps extensions (e.g., `.mp4` -> `video`, `.png` -> `image`, `.pdf` -> `doc`, `.zip` -> `archive`).
* **Q75: What sections can users toggle between in MediaVault?**
  *Answer*: `upload`, `library`, `activity`, and `settings` via `currentSection` state.
* **Q76: How does search filtering work in `FileLibrary.tsx`?**
  *Answer*: Filters `driveFiles` against `searchQuery` and `currentFilter` (all, images, videos, documents).
* **Q77: How does MediaVault support external Google Drive linking?**
  *Answer*: Users configure a drive link in `SettingsPanel.tsx`, which persists in storage and opens in a new tab.
* **Q78: How is file deletion handled?**
  *Answer*: `deleteFile(id)` filters out the target ID from `driveFiles`, writes the updated array to storage, and logs a `delete` action.
* **Q79: What is the purpose of `Ticker.tsx`?**
  *Answer*: A running header ticker displaying real-time platform diagnostics, versioning (`v2.0.0`), and storage statistics.
* **Q80: Why was Zustand chosen over Redux for MediaVault?**
  *Answer*: Zustand has a tiny 1.1 KB footprint, requires no provider nesting, and supports direct selector subscriptions to prevent re-renders.
* **Q81: What happens if an analyst uploads an external URL?**
  *Answer*: `addUrlToQueue` parses the URL slug, assigns an inferred file type, and stages it for vault download.
* **Q82: How does MediaVault generate unique IDs?**
  *Answer*: Uses `uid()`, combining a timestamp with random base-36 characters (`Math.random().toString(36)`).
* **Q83: Why is an activity audit log critical in forensics?**
  *Answer*: Proves chain of custody in legal proceedings by demonstrating exactly when evidence was ingested or accessed.
* **Q84: How would MediaVault scale beyond local storage limits?**
  *Answer*: Connect the upload pipeline to AWS S3 configured with Object Lock (WORM storage) and use pre-signed URLs.
* **Q85: What happens when `clearQueue` is called?**
  *Answer*: Clears all pending staging items while leaving committed drive files intact.

---

## 6. Executive Intelligence & Credibility Scoring (Q86 – Q105)

* **Q86: What is the VeritAI Executive Dashboard?**
  *Answer*: A 1,685-line analytical workstation in `VeritAIApp.tsx` displaying media credibility ratings, bias scores, and claim audits.
* **Q87: What are the four core sub-indexes calculated in VeritAI?**
  *Answer*: `fc` (Factual Consistency), `sr` (Source Reliability), `sb` (Sensationalism & Clickbait), and `cb` (Cognitive Balance).
* **Q88: What is Media Bias/Fact Check (MBFC)?**
  *Answer*: An independent media rating agency used as an editorial standard to categorize sources into reliability tiers.
* **Q89: What reliability tiers does VeritAI display?**
  *Answer*: HIGH RELIABILITY (green), MED RELIABILITY (amber), and NO RELIABILITY (red).
* **Q90: How does VeritAI highlight text manipulation?**
  *Answer*: Uses colored spans (`hl-good` for verified facts, `hl-bad` for emotional editorializing, `hl-claim` for unverified statements).
* **Q91: What claim verdicts can be assigned?**
  *Answer*: `VERIFIED` (green), `DISPUTED` (red), and `UNVERIFIED` (blue).
* **Q92: What fact-checking sources are integrated?**
  *Answer*: PolitiFact, FactCheck.org, Snopes, AP Fact Check, and Reuters Fact Check.
* **Q93: How is emotional language percentage calculated?**
  *Answer*: By measuring the density of sensational, alarmist, and emotionally charged keywords relative to neutral factual reporting.
* **Q94: What does the political bias indicator show?**
  *Answer*: Source editorial leanings, ranging from CENTER to RIGHT-CENTER, LEFT, or CONSPIRACY.
* **Q95: How does the theme toggle work in `VeritAIApp.tsx`?**
  *Answer*: Managed via `ThemeCtx`, toggling between NVIDIA dark styling and high-contrast light mode.
* **Q96: How does the live scan simulation work?**
  *Answer*: Animates a radar sweep line and progress bar before revealing calculated credibility metrics.
* **Q97: What alerts are rendered for low-credibility sources?**
  *Answer*: Warnings highlighting extreme emotional language (e.g., 82%), disputed factual assertions, and lack of editorial standards.
* **Q98: What font families are used in VeritAI?**
  *Answer*: `'Share Tech Mono'` for forensic metadata, numbers, and badges; `'Barlow Condensed'` for high-impact executive headings.
* **Q99: What is the badge styling pattern in VeritAI?**
  *Answer*: `getBadgeStyle` generates a glowing translucent background, matching text color, and subtle 1px border.
* **Q100: How are claims verified?**
  *Answer*: Cross-referenced against known fact-checking databases with specific source ratings (e.g., PolitiFact "PANTS ON FIRE").
* **Q101: What does a high clickbait score indicate?**
  *Answer*: The headline uses emotional manipulation, curiosity gaps, or sensationalist claims unsupported by the article text.
* **Q102: How does VeritAI handle analyst predictions?**
  *Answer*: Labels them as `UNVERIFIED` analyst opinion rather than flagging them as malicious falsehoods.
* **Q103: How does the dashboard integrate with Supabase Auth?**
  *Answer*: It imports `useAuth()` to retrieve the logged-in analyst's name, initials avatar, and access tier.
* **Q104: What is the advantage of pre-calibrated demo profiles?**
  *Answer*: Provides instant demonstrations of high, medium, and low credibility scenarios for training and evaluation.
* **Q105: How does VeritAI display executive summaries?**
  *Answer*: Renders a concise description explaining the exact reasoning behind the composite credibility score.

---

## 7. Authentication, Security & Database (Q106 – Q125)

* **Q106: How is user authentication implemented?**
  *Answer*: Through Supabase GoTrue, supporting email/password authentication and federated Google OAuth via Lovable.
* **Q107: What database table stores user metadata?**
  *Answer*: The `profiles` table in Supabase PostgreSQL, linked to `auth.users(id)` via `user_id`.
* **Q108: What fields exist in the `profiles` table?**
  *Answer*: `id`, `user_id`, `display_name`, `avatar_url`, `role`, `provider`, `created_at`, and `updated_at`.
* **Q109: How does `AuthContext.tsx` maintain session state?**
  *Answer*: Listens to `supabase.auth.onAuthStateChange` and updates React state with session JWTs stored in `localStorage`.
* **Q110: How are avatar initials generated?**
  *Answer*: Extracts the first letter of the first two words in `display_name` and converts them to uppercase (`slice(0, 2)`).
* **Q111: What system roles are supported?**
  *Answer*: `Journalist`, `Analyst`, and `Admin`.
* **Q112: How does Row Level Security (RLS) protect profiles?**
  *Answer*: PostgreSQL policies ensure `auth.uid() = user_id`, preventing unauthorized users from reading or editing other profiles.
* **Q113: Why is password hashing handled by Supabase rather than custom code?**
  *Answer*: Supabase uses industry-standard Bcrypt/Argon2 with automatic salt generation, eliminating custom security vulnerabilities.
* **Q114: What token refresh strategy is used?**
  *Answer*: The Supabase client automatically refreshes expiring JWT access tokens using background refresh tokens.
* **Q115: How does the application protect against XSS?**
  *Answer*: React automatically encodes strings rendered in JSX, and external links explicitly use `rel="noopener noreferrer"`.
* **Q116: How does the application prevent SQL injection?**
  *Answer*: PostgREST uses parameterized SQL queries for all database interactions.
* **Q117: What is the purpose of `TooltipProvider` in `App.tsx`?**
  *Answer*: Provides an accessible Radix UI context for tooltips across all interactive buttons.
* **Q118: How does the app handle logout?**
  *Answer*: Calls `supabase.auth.signOut()` and resets user and session states to `null`.
* **Q119: What environment variables are required for Supabase?**
  *Answer*: `VITE_SUPABASE_URL` and `VITE_SUPABASE_PUBLISHABLE_KEY`.
* **Q120: Why is it safe to expose `VITE_SUPABASE_PUBLISHABLE_KEY` in frontend bundles?**
  *Answer*: It is an anonymous public key restricted by PostgreSQL Row Level Security policies.
* **Q121: How are Google OAuth logins initiated?**
  *Answer*: Via `lovable.auth.signInWithOAuth("google", { redirect_uri: window.location.origin })`.
* **Q122: What happens if profile lookup fails?**
  *Answer*: Falls back to the username derived from the email address (`email.split("@")[0]`) and assigns the default `Journalist` role.
* **Q123: How are auth subscription listeners cleaned up?**
  *Answer*: The `useEffect` hook returns `() => subscription.unsubscribe()` to prevent memory leaks on unmount.
* **Q124: How does TruthGuard defend against CSRF attacks?**
  *Answer*: Supabase Auth stores tokens in `localStorage` and transmits them via `Authorization: Bearer` headers rather than ambient cookies.
* **Q125: What is the role of `Sonner` in `App.tsx`?**
  *Answer*: An alternative toast notification system offering high-performance, stacked alerts for system notifications.

---

## 8. AI Pipeline, Prompting & Grounding (Q126 – Q140)

* **Q126: Why does TruthGuard use pre-trained AI APIs instead of training a custom model?**
  *Answer*: Pre-trained foundation models adapt instantly to new diffusion architectures, whereas custom models require millions in compute and risk rapid obsolescence.
* **Q127: What is the execution runtime for Supabase Edge Functions?**
  *Answer*: Deno V8 running on Cloudflare/Deno global edge infrastructure with sub-50ms cold starts.
* **Q128: How does TruthGuard enforce cost control on AI calls?**
  *Answer*: By truncating text inputs to 5,000 characters and performing image EXIF and hash audits client-side for free.
* **Q129: What is prompt injection, and how is it mitigated?**
  *Answer*: When malicious inputs attempt to override system instructions; mitigated via length slicing, strict JSON output schemas, and factual cross-checks.
* **Q130: How does TruthGuard handle LLM hallucinations?**
  *Answer*: By grounding extracted claims against verified fact-checking syndications (PolitiFact, Snopes, Reuters) with direct citation links.
* **Q131: What is the fallback mechanism if an AI Edge Function times out?**
  *Answer*: The edge handler falls back to deterministic regex pattern matching, and the client displays a non-blocking toast alert.
* **Q132: What structured JSON format does `analyze-media` return?**
  *Answer*: An object containing `verdict`, `confidence`, `summary`, `details`, and `factCheckSources`.
* **Q133: What is the purpose of `AnalyzingOverlay.tsx`?**
  *Answer*: Renders an animated radar sweep while the user waits for Edge Function completion, providing visual feedback.
* **Q134: How are fact-checking sources grounded?**
  *Answer*: Each verified claim includes source name, verdict rating, and the original investigation URL.
* **Q135: Why are edge functions better than centralized servers for inference?**
  *Answer*: Edge workers execute close to the analyst globally, reducing round-trip latency and scaling automatically.
* **Q136: How does TruthGuard avoid vendor lock-in?**
  *Answer*: The frontend calls a generic edge endpoint (`analyze-media`), allowing backend models to be swapped without changing client code.
* **Q137: What is the difference between authentic, suspicious, and fake verdicts?**
  *Answer*: Authentic (>=70% verified factual), Suspicious (unverified claims or mixed signals), Fake (<40% credibility with confirmed synthetic artifacts).
* **Q138: How does TruthGuard prevent token exhaustion attacks?**
  *Answer*: Enforces strict string length slicing (`slice(0, 5000)`) before payloads leave the browser.
* **Q139: What is the role of `StatsBar.tsx` in TruthLence?**
  *Answer*: Aggregates total scans and displays real-time counters for authentic, suspicious, and fake files analyzed in that session.
* **Q140: Can an analyst download the browser extension from TruthLence?**
  *Answer*: Yes, clicking "Download Extension" fetches `/truthlens-extension.zip` for real-time web verification.

---

## 9. Follow-Up Interview Chains & Reflections (Q141 – Q150)

* **Q141: "Why Supabase over MongoDB?"**
  *Answer*: Supabase provides PostgreSQL with relational integrity, foreign keys, and Row Level Security, alongside serverless Edge Functions. MongoDB requires building custom authentication servers and lacks built-in row-level authorization.
* **Q142: "What if TruthGuard needs to store 50 petabytes of video evidence? How would you migrate?"**
  *Answer*: I would decouple asset storage from PostgreSQL. Video bytes would stream directly to AWS S3 buckets configured with Object Lock (WORM compliance), while PostgreSQL would store only cryptographic SHA-256 hashes, metadata, and pre-signed access URLs.
* **Q143: "What happens if two analysts upload the exact same file simultaneously?"**
  *Answer*: Both files compute the identical SHA-256 hash. In our planned Redis caching layer, the second upload would return the cached verdict instantly with zero redundant AI inference cost.
* **Q144: "How do you trace a production bug live?"**
  *Answer*: Inspect browser DevTools Network tab for failed Edge Function status codes, review Supabase Edge Function execution logs in the cloud console, and verify localStorage state in the Application tab.
* **Q145: "How did you manage technical scope during development?"**
  *Answer*: Prioritized core forensic engines (EXIF parsing, Web Crypto hashing, and Edge AI classification) first, before adding advanced UI micro-animations and port probing.
* **Q146: "What did you learn about client-side performance during this project?"**
  *Answer*: That native browser APIs like Web Crypto and `exifr` are dramatically faster than JavaScript-heavy npm packages, and that high-frequency progress updates must be decoupled from React Context.
* **Q147: "What was your proudest moment building TruthGuard?"**
  *Answer*: Getting the dynamic port prober in `FeatureEmbed.tsx` working seamlessly with `HEAD` and `no-cors` mode, turning disconnected local dev servers into a cohesive federated platform.
* **Q148: "How would you add collaborative multi-analyst case sharing?"**
  *Answer*: By enabling Supabase Realtime WebSocket subscriptions on an `incidents` table, allowing multiple analysts to view live case updates and comments.
* **Q149: "How would you handle rate limiting on the Edge Functions?"**
  *Answer*: Deploy an Upstash Redis rate limiter using a sliding window counter algorithm inside the Deno edge worker, returning HTTP 429 when limits are exceeded.
* **Q150: "What questions do you have for our engineering team?"**
  *Answer*: "How does your team handle live evidence custody in your SOC pipelines, and do you currently run computer vision inference client-side or offload entirely to distributed cloud clusters?"
