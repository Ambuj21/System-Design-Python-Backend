# Study Progress — System Design + Python Backend

## Placement
- Phase: 1 (Python backend mastery) — in progress
- Level within phase: Intermediate (defaulted). Phase 0 diagnostic was explicitly skipped by learner request on 2026-08-19 — no calibration data exists. Re-run or spot-check placement if early lessons land oddly easy or oddly hard.

## Topics covered

### 1. The GIL & the concurrency model — threading vs multiprocessing vs asyncio (2026-08-19)
- What the GIL actually locks: one thread executing Python **bytecode** at a time, not "one thread at a time" full stop.
- When it's released: blocking syscalls (I/O), `time.sleep`/`asyncio.sleep`, and C extensions (NumPy/pandas) that drop it during their C loop.
- Round-robin GIL hand-off mechanics and the forced-yield switch interval (why CPU-bound threading has real overhead, not just zero benefit).
- Measured (this session, 12-core box, CPython 3.10.12): CPU-bound 4 workers/15M-iter loop — sequential 1.953s, threading **5.868s (slower than sequential)**, multiprocessing 0.548s. I/O-bound 8 workers/0.3s delay — sequential 2.403s, threading 0.304s, asyncio 0.302s.
- Why asyncio does nothing for CPU-bound work (no await point = no yield = strictly sequential).
- Event-loop blocking failure mode: one synchronous call inside an async handler freezes every other in-flight coroutine on that worker.
- Real-world grounding: gunicorn/uvicorn hybrid worker model (processes for cores + event loop for I/O concurrency within each), NumPy/pandas releasing the GIL, CPython 3.13 free-threaded build (PEP 703), Redis's single-threaded command loop as the same mechanism used deliberately.
- Lesson page: **`lessons/01-gil-concurrency-model.html`** — self-contained, theme-aware (`prefers-color-scheme`), local HTML lesson with the interactive GIL-hand-off/process-parallelism/event-loop simulator (CPU-bound vs I/O-bound, plus a "inject a blocking call" gotcha mode), the real benchmark bars, and the full written lesson (hook, analogy, real-world grounding, hands-on code walkthrough, trade-offs, understanding-check). Browse via `lessons/index.html`.
  - *Delivery format changed mid-session*: this topic was originally built and taught as an externally-hosted Artifact (published at `https://claude.ai/code/artifact/62d4f9f3-ed83-4e09-b73c-8c0650f24810`). The learner requested a switch to a local, self-contained lesson site instead — that Artifact URL is now retired/non-canonical. `lessons/01-gil-concurrency-model.html` is the canonical version going forward; all future lessons are built directly in this format (see the agent's own spec file for the "Lesson site" rules).
- Understanding check posed: "Teammate wants to rewrite a Django view (200ms across 3 sequential external API calls, 5ms own logic) using `threading`. Right call? What changes if those 3 calls were CPU-heavy image transforms instead?" — **response pending**, evaluate first thing next session.

## Lesson library
Bulk-generated on **2026-08-19**: 34 new lesson files were written into `lessons/` alongside the pre-existing Lesson 01, giving a complete 35-lesson reference library for this curriculum (`lessons/index.html` is the table of contents). **Generated does not mean learned.** Only Lesson 01 has actually been taught interactively — full walkthrough, understanding-check posed — as logged under "Topics covered" above. Lessons 02–35 exist on disk and are validated to render, but none of them have been taught, walked through, or understanding-checked yet. Their presence here is inventory, not progress; do not count them as covered material or skip the interactive teaching flow because the file already exists.

### Phase 1 — Python backend mastery
1. `lessons/01-gil-concurrency-model.html` — The GIL & the Concurrency Model *(taught — see Topics covered above)*
2. `lessons/02-async-event-loop.html` — Async/Await Mechanics & the Event Loop
3. `lessons/03-fastapi-django-internals.html` — FastAPI & Django Internals
4. `lessons/04-rest-graphql-api-design.html` — REST & GraphQL API Design
5. `lessons/05-orm-database-internals.html` — ORM & Database Internals
6. `lessons/06-redis-caching-patterns.html` — Redis Caching Patterns
7. `lessons/07-testing-strategy.html` — Testing Strategy for Backend Systems
8. `lessons/08-profiling-performance.html` — Profiling & Performance
9. `lessons/09-packaging-deployment.html` — Packaging & Deployment
10. `lessons/10-observability.html` — Observability

### Phase 2 — System design fundamentals
11. `lessons/11-scalability-fundamentals.html` — Scalability Fundamentals
12. `lessons/12-load-balancing.html` — Load Balancing
13. `lessons/13-caching-strategies-invalidation.html` — Caching Strategies & Invalidation at the Architecture Level
14. `lessons/14-database-scaling-replication-sharding.html` — Database Scaling: Replication & Sharding
15. `lessons/15-cap-theorem-pacelc.html` — CAP Theorem & PACELC
16. `lessons/16-consistency-models.html` — Consistency Models
17. `lessons/17-message-queues-streaming.html` — Message Queues & Streaming
18. `lessons/18-microservices-vs-monolith.html` — Microservices vs Monolith
19. `lessons/19-api-gateways-service-mesh.html` — API Gateways & Service Mesh
20. `lessons/20-rate-limiting-algorithms.html` — Rate Limiting Algorithms
21. `lessons/21-cdns.html` — CDNs
22. `lessons/22-consensus-raft-paxos.html` — Consensus Basics (Raft/Paxos)
23. `lessons/23-designing-for-failure.html` — Designing for Failure

### Phase 3 — Applied system design in Python
24. `lessons/24-build-rate-limiter.html` — Build a Rate Limiter in Python
25. `lessons/25-build-lru-cache.html` — Build an LRU/LFU Cache in Python
26. `lessons/26-build-consistent-hash-ring.html` — Build a Consistent-Hash Ring in Python
27. `lessons/27-build-pubsub-system.html` — Build a Simple Pub/Sub System in Python
28. `lessons/28-build-leader-election.html` — Build a Toy Leader-Election Algorithm in Python

### Phase 4 — Case studies
29. `lessons/29-design-url-shortener.html` — Design a URL Shortener
30. `lessons/30-design-news-feed.html` — Design a News Feed (Twitter-like)
31. `lessons/31-design-ride-dispatch.html` — Design a Ride-Dispatch System (Uber-like)
32. `lessons/32-design-video-streaming.html` — Design a Video Streaming Platform (Netflix-like)
33. `lessons/33-design-messaging-system.html` — Design a Messaging System (WhatsApp-like)
34. `lessons/34-design-payments-system.html` — Design a Payments System
35. `lessons/35-design-distributed-rate-limiter-cache.html` — Design a Distributed Rate Limiter/Cache at Scale

## Weak spots (spaced-repetition queue)
(none flagged yet — only one topic covered so far; populate after the pending understanding-check is scored)

## Last session
2026-08-19 — Session 1. Learner explicitly skipped the Phase 0 diagnostic and asked to go straight into topic-by-topic teaching with a dedicated animated/interactive visual per topic, in depth. Placement defaulted to intermediate. Taught GIL/concurrency model (Phase 1, topic 1) via the full 8-step flow, initially as an externally-hosted Artifact; mid-session the learner switched the delivery format to a local, self-contained lesson site, so the lesson was retrofitted into `lessons/01-gil-concurrency-model.html` plus a new `lessons/index.html` course homepage, and the agent's own spec file (`.claude/agents/system-design-python-mentor.md`) now mandates this local format for all future lessons. Spaced-recap step was N/A (no prior topics exist yet). Understanding-check question posed in both the lesson page and chat; learner's answer not yet given.

## Next session recommendation
Score the learner's pending Lesson 01 understanding-check first (I/O-bound sequential API calls vs CPU-bound image transforms — threading vs multiprocessing vs asyncio). Log the result as solid/shaky/weak and file any gap into the weak-spots queue before moving on. Then proceed interactively through the lesson library in curriculum order — **Lesson 02 (`lessons/02-async-event-loop.html`) next** — continuing to log real understanding-check results and weak spots per lesson as they're actually taught. The lesson library being pre-built ahead of schedule does not skip the interactive teaching flow: each lesson still needs to be walked through live, with its own understanding-check posed and scored, before it counts as covered in "Topics covered" above.
