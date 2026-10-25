# Project Pitch: TruthGuard Platform

## 1. The 30-Second Elevator Pitch
*(Perfect for the opening question: "Tell me about your project in 30 seconds.")*

> "I built TruthGuard, an integrated cyber-defense and digital media forensics platform designed to detect generative AI deepfakes, surveillance camera tampering, and voice phishing scams.
> 
> Rather than relying on a single static check, I architected a federated micro-frontend suite connecting five specialized engines—combining client-side EXIF metadata parsing and Web Crypto SHA-256 hashing with serverless edge AI functions for multi-modal verification.
> 
> The platform unifies image error level analysis, telecommunication scam triage, and cryptographic asset provenance into an NVIDIA-inspired dark workstation.
> 
> It solves the growing crisis of synthetic media by giving security analysts instant, multi-layered evidence verification with definitive confidence scores."

---

## 2. The 2-Minute Comprehensive Pitch
*(Tailored for: "Walk me through what you built, why you built it, and the architecture.")*

> "With the recent explosion of generative AI—such as diffusion image generators, ElevenLabs voice cloning, and automated phishing bots—traditional security perimeters are completely blind to synthetic media. Bad actors can now bypass biometric gates, spoof executive phone calls, and tamper with physical CCTV feeds. I built TruthGuard to give security teams and journalists a single, unified forensics workstation to detect and mitigate these multi-modal threats in real time.
> 
> Architecturally, TruthGuard is structured as a federated micro-frontend platform. At its core is a central Hub built with React 18, TypeScript, and Vite. The Hub coordinates five specialized forensic engines:
> 
> First, **TruthLence**, which inspects uploaded imagery, documents, and news URLs for generative compression artifacts, cross-referencing claims against established fact-checking sources through Supabase Edge Functions.
> 
> Second, **DEEP SCAN**, an optical tamper and camera forensics engine. It processes media right in the browser using the Web Crypto API to calculate SHA-256 hashes and `exifr` to extract detailed camera metadata, GPS coordinates, and detect whether editing software like Photoshop, GIMP, or Canva altered the image.
> 
> Third, **DeepGuard**, which triages telecommunication threats. It analyzes phone call transcripts and SMS messages for voice synthesis patterns, artificial urgency, and smishing attacks.
> 
> Fourth, **MediaVault**, a cryptographic asset custody drive built with Zustand that handles file ingestion, simulated multi-stage encryption, and immutable activity audit logging.
> 
> And fifth, the **VeritAI Executive Dashboard**, a 1,600-line analytical command center featuring radial confidence gauges, source reliability tiers based on Media Bias/Fact Check standards, and emotional sensationalism meters.
> 
> One engineering highlight I'm proud of is our client-side dynamic port prober in `FeatureEmbed.tsx`. It actively probes localhost ports using non-blocking HEAD requests to discover running micro-frontend instances dynamically without requiring a hardcoded backend registry.
> 
> Overall, TruthGuard combines client-side cryptographic speed with serverless AI intelligence to provide deep, explainable, and trustworthy forensics against modern synthetic threats."
