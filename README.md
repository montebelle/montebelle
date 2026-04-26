### J. T. Bell

I build production forecasting, causal-inference, and agentic systems.

The biggest thing I'm building is **Montebelle** — a managed-agent platform that runs autonomous agents in the messaging apps clients already use (WhatsApp, Slack, Signal, Discord). Alongside it: an on-device meeting copilot, a fully-local batch RAG pipeline, a career-intelligence skill for Claude Code, and a small body of technical writing. All of it is below.

**Built for myself first.** Every public system here started as something I run daily. If it survives a year of use, it goes public.

**Speed without judgment is liability.** Each repo has the verification step — dedup, calibration, gates, bias audit, human approval. Confident systems emitting nonsense is the failure mode I optimize against.

---

### Selected projects

**[meetings](https://github.com/montebelle/meetings)** — Real-time meeting copilot on a single 24 GB M4 Pro.
MLX Whisper large-v3-turbo for transcription + Qwen3.5-9B (262K ctx) for response generation, with **bidirectional GPU eviction** — the two cannot coexist in 24 GB, so Whisper is unloaded before LLM generation, then the LLM is unloaded before the next Whisper pass. 8-bit KV-cache quantization, speculative decoding, 6-strategy hallucination filter, bandpass VAD (300–3400 Hz). Context loader auto-switches from direct prompt injection to LanceDB + nomic RAG at 400K chars. FastAPI + WebSocket, single-page UI.

**[recordings](https://github.com/montebelle/recordings)** — Fully-local 5-step batch RAG (transcribe → extract → notes → embed → summarize).
3-level LanceDB hierarchy (chunk / section / document summary). Sentence-aware chunking — target 500 words, min 200, max 700, with 2-sentence overlap between chunks. Hybrid vector + BM25 with **RRF k=60** (Bruch et al, ACM TOIS 2024 — +1.4% nDCG over dense, +18% over BM25 on BEIR/MS MARCO). Query reformulation for follow-ups, MCP server for IDE integration. 100% on-device.

**[jobe-skill](https://github.com/montebelle/jobe-skill)** — Career intelligence skill for Claude Code.
- **MinHash LSH dedup** (lib/minhash.js, lib/dedup.js) — LSHBloom defaults: 128 perms, 18×7 bands, Jaccard ≥ 0.70 on bigram shingles
- **RRF k=60 hybrid ranking** across quick + full + freshness signals (Bruch et al 2024)
- **Multi-signal ghost-job detection** — age, repost cadence, company-ratio, layoff signals, title fuzziness (Clarify Capital 2024 n=1,200 + Hunter Ng arXiv 2410.21771)
- **LLM-judge calibration** with Cohen's κ ≥ 0.75 flag (arXiv 2506.13639, 2025)
- **Bias audit** via name + school perturbations (Bertrand & Mullainathan AER 2004 + Brookings 2024)
- 8 discovery sources, 30-day cache, lazy enrichment

**[openmbps-platform](https://github.com/montebelle/openmbps-platform)** — Minimal framework for autonomous agent operations on a single machine.
Cron-driven, filesystem-backed, human-approved at publish gates. 28-cron orchestration with deterministic dedup keys, bounded retries, and explicit approval signals — designed so a single operator can fan out across multiple client workspaces without losing the audit trail.

**[writings](https://github.com/montebelle/writings)** — Technical essays on forecasting, causal inference, and agent-system design.
