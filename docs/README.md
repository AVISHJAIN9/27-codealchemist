# TruthGuard Platform: Technical Documentation

Welcome to the comprehensive, interview-ready documentation suite for **TruthGuard: Unified Cyber-Defense, Deepfake Forensics & Optical Tamper Platform** (`33-codealchemist`).

This documentation is crafted in the style of the Vue.js documentation: clear, modular, step-by-step, accompanied by architecture diagrams, exhaustive technical comparisons, spoken interview scripts, and a full system design curriculum.

---

## Complete Reading Order Index

### 1. Foundation & Platform Discovery
- [00. Project Type Classification](./00-project-type.md): Project stack detection (React 18 + TS + Vite + Supabase Edge Functions + Micro-Frontend Hub).
- [00a. Full Project Analysis](./00a-project-analysis.md): System topology, user journeys, route catalog, full feature traces (F1–F6), inventory counts, and cross-micro-frontend connections.
- [00b. Project Pitch (30-Second & 2-Minute Scripts)](./00b-project-pitch.md): Spoken elevator pitches tailored for hiring managers, technical interviewers, and senior architects.
- [00. Comprehensive Project Map](./00-project-map.md): Annotated directory tree, dependency audit, environment variable registry, external services, and runtime entry points.

### 2. Feature Deep Dives & Mechanics
- [01. Feature Inventory & Matrix](./01-features.md): Overview matrix connecting features, micro-frontend ports, cryptographic primitives, and backend edge functions.
- [Feature F1: TruthLence Deepfake Image Forensics](./features/F1-truthlence-deepfake-image-forensics.md): Error Level Analysis (ELA), edge discrepancy metrics, diffusion blurring detection, and spoken defense.
- [Feature F2: Camera Tamper & Optical Obstruction Guard](./features/F2-camera-tamper-optical-obstruction-guard.md): Real-time optical flow monitoring, canvas-based variance analysis, stream freeze alerts, and spoken defense.
- [Feature F3: Scam Call & SMS Phishing Detector](./features/F3-scam-call-sms-phishing-detector.md): Heuristic urgency token scanning, natural language sentiment evaluation, phishing URL regex inspection, and spoken defense.
- [Feature F4: MediaVault Cryptographic Provenance](./features/F4-mediavault-cryptographic-provenance.md): SHA-256 asset hashing, EXIF metadata extraction, immutable chain-of-custody recording, and spoken defense.
- [Feature F5: Unified Hub Threat Intelligence Dashboard](./features/F5-unified-hub-threat-intelligence-dashboard.md): Active cross-port prober, embedded iframe sandbox orchestration, aggregated security event logging, and spoken defense.

### 3. Engineering Implementation & API Reference
- [02. Step-by-Step Build Order](./02-build-order.md): 6-phase reconstruction guide from empty directory to full federated production deployment with line-by-line code explanations.
- [03. API & Routes Reference](./03-api-reference.md): Complete specifications for Supabase Edge Functions (`analyze-media`, `analyze-content`), PostgreSQL REST endpoints, authentication protocols, and cross-port health check probing.
- [04. Exhaustive Tech Choice Comparisons](./04-tech-comparisons.md): 10-point comparison tables comparing every selected technology (React, Vite, Supabase, Tailwind, Zustand, Web Crypto API) against industry alternatives.

### 4. Architecture, Security & AI
- [05. System Architecture & Hub Topology](./05-architecture.md): Federated micro-frontend architecture, top 3 request lifecycles, database ER diagram, auth flow, and 100x scaling bottlenecks.
- [06. Security Audit & Threat Matrix](./06-security.md): OWASP Top 10 compliance audit, zero-trust evidence handling, RLS policies, iframe sandbox confinement, and prompt injection defense.
- [07. AI Integration & Forensic Pipeline](./07-ai-integration.md): Pre-trained vision/language models vs custom ML training, prompt engineering, structured JSON outputs, anti-hallucination grounding, and token cost economics.
- [08. Engineering Quality & Operations](./08-quality-and-ops.md): Vitest unit testing, CI/CD GitHub Actions workflows, performance bottlenecks, concurrency risks, and technical debt log.

### 5. System Design Deep Dive (Chapters 14.1 – 14.17)
- [System Design Overview & Index](./system-design/index.md): System design roadmap and architecture principles.
- [14.1 Requirements & Capacity Estimation](./system-design/14.1-requirements-and-estimation.md): Functional/non-functional specs, QPS math, storage sizing, and AI token economics.
- [14.2 Architecture Styles](./system-design/14.2-architecture-styles.md): Federated micro-frontends vs monolithic SPA vs microservices.
- [14.3 Networking & Communication](./system-design/14.3-networking-and-communication.md): HTTP/2, TLS 1.3, cross-port probing, and iframe messaging.
- [14.4 Load Balancing & Scaling](./system-design/14.4-load-balancing-and-scaling.md): L7 ingress routing, horizontal autoscaling, and eliminating SPOFs.
- [14.5 Databases & Storage Architecture](./system-design/14.5-databases-and-storage.md): PostgreSQL vs NoSQL/NewSQL, indexing, and object storage tiers.
- [14.6 Multi-Tier Caching Strategies](./system-design/14.6-caching.md): Browser, CDN, application, and SHA-256 semantic Redis caching.
- [14.7 Messaging, Queues & Async Processing](./system-design/14.7-messaging-and-queues.md): BullMQ task queues, asynchronous forensic pipelines, and DLQ patterns.
- [14.8 Consistency, Concurrency & Distributed Systems](./system-design/14.8-consistency-and-concurrency.md): ACID guarantees, optimistic locking, and distributed idempotency.
- [14.9 Reliability, Resilience & Chaos Engineering](./system-design/14.9-reliability-and-resilience.md): Circuit breakers, retry with exponential backoff and jitter, and SLAs.
- [14.10 API Design & Rate Limiting Architecture](./system-design/14.10-api-design-and-rate-limiting.md): Token bucket algorithm, cursor pagination, and API gateway protection.
- [14.11 Enterprise Security Architecture](./system-design/14.11-security-architecture.md): Zero-trust principles, RLS data isolation, and cryptographic integrity.
- [14.12 Search & AI Architecture](./system-design/14.12-search-and-ai-architecture.md): `pgvector` hybrid search, prompt orchestration, and anti-hallucination grounding.
- [14.13 Real-Time Systems & Stream Ingestion](./system-design/14.13-real-time-systems.md): SSE vs WebSockets and Redis pub/sub backplanes.
- [14.14 Observability & Site Reliability Engineering](./system-design/14.14-observability-and-operations.md): Structured logging, OpenTelemetry tracing, and canary deployments.
- [14.15 Performance Engineering & Web Vitals](./system-design/14.15-performance-engineering.md): Canvas pixel loop optimization, LCP/INP/CLS metrics, and code splitting.
- [14.16 Cost Modeling & Cloud Economics](./system-design/14.16-cost-and-trade-offs.md): Unit cost economics ($46k/mo for 100k DAU) and vendor lock-in exit plans.
- [14.17 Classic System Design Scenarios](./system-design/14.17-classic-system-design.md): 100x scaling, notification engines, distributed media ingest, and URL shortener reuse.
- [Architecture Decision Records (ADR)](./system-design/decisions.md): Exhaustive log of all 10 core architectural decisions, alternatives, trade-offs, and trigger conditions.

### 6. Interview Preparation & Verification
- [09. Interview Q&A (150+ Questions)](./09-interview-qa.md): Categorized questions spanning architecture, React internals, security, databases, AI integration, and behavioral reflection.
- [10. Gaps & Engineering Roadmap](./10-gaps-and-improvements.md): Prioritized technical gaps with concrete production-ready code fixes.
- [11. Technical & Forensic Glossary](./11-glossary.md): Clear definitions for all cryptographic, forensic, computer vision, and distributed systems terms.
- [12. Confirm List](./12-confirm-list.md): Audit catalog of heuristic thresholds and implementation assumptions marked `[CONFIRM WITH DEVELOPER]`.
- [13. Master Checklist (275 Items)](./13-master-checklist.md): Comprehensive 7-part audit of all 275 items with a full coverage status matrix.
- [14. Say It Out Loud Spoken Master Scripts](./14-say-it-out-loud.md): Unified spoken scripts for all features, 30s/2min pitch, and interactive Rehearsal Checklist.

---

## Running the Documentation Locally

To launch the VitePress documentation server locally:

```bash
# Start local VitePress documentation server
npm run docs:dev

# Build production static bundle
npm run docs:build

# Preview production build locally
npm run docs:preview
```

