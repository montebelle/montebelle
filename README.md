### J. T. Bell

I build production forecasting, causal inference, and agentic systems.

The way I build them is the part I think hardest about. Speed without judgment is liability. Confident systems emitting nonsense is the failure mode I optimize against. So every system I ship has the verification step inside the loop. Dedup. Calibration. Gates. Bias audit. Human approval. Not bolted on after the fact.

Montebelle is where that ethos gets the most direct expression. It is not a business. It is a project I run on myself first. An agent platform that lives in the messaging apps people already use (WhatsApp, Slack, Signal, Discord), held to the same gates as the rest of my work. Built for myself. Deployed for others only after a year of survival in my own daily use.

The other repos here are the same idea in other shapes. A real-time meeting copilot. A fully local batch RAG pipeline. A career intelligence skill for Claude Code. A small body of technical writing. Different surfaces. Same principle.

---

### Selected projects

**[meetings](https://github.com/montebelle/meetings)**: real-time meeting copilot on a single 24 GB M4 Pro.
MLX Whisper large-v3-turbo for transcription, plus Qwen3.5-9B (262K ctx) for response generation, with **bidirectional GPU eviction**. The two cannot coexist in 24 GB, so Whisper is unloaded before LLM generation, then the LLM is unloaded before the next Whisper pass. 8-bit KV cache quantization, speculative decoding, 6-strategy hallucination filter, bandpass VAD between 300 and 3400 Hz. Context loader auto-switches from direct prompt injection to LanceDB plus nomic RAG at 400K chars. FastAPI, WebSocket, single-page UI.

**[recordings](https://github.com/montebelle/recordings)**: fully local 5-step batch RAG (transcribe, extract, notes, embed, summarize).
3-level LanceDB hierarchy (chunk, section, document summary). Sentence-aware chunking with target 500 words, minimum 200, maximum 700, and 2-sentence overlap between chunks. Hybrid vector plus BM25 with **RRF k=60** (Bruch et al, ACM TOIS 2024; +1.4% nDCG over dense, +18% over BM25 on BEIR and MS MARCO). Query reformulation for follow-ups, MCP server for IDE integration. 100% on-device.

**[jobe-skill](https://github.com/montebelle/jobe-skill)**: career intelligence skill for Claude Code.
- **MinHash LSH dedup** (lib/minhash.js, lib/dedup.js). LSHBloom defaults: 128 perms, 18×7 bands, Jaccard ≥ 0.70 on bigram shingles
- **RRF k=60 hybrid ranking** across quick, full, and freshness signals (Bruch et al 2024)
- **Multi-signal ghost-job detection** across age, repost cadence, company ratio, layoff signals, and title fuzziness (Clarify Capital 2024 n=1,200; Hunter Ng arXiv 2410.21771)
- **LLM-judge calibration** with Cohen's κ ≥ 0.75 flag (arXiv 2506.13639, 2025)
- **Bias audit** via name and school perturbations (Bertrand and Mullainathan AER 2004; Brookings 2024)
- 8 discovery sources, 30-day cache, lazy enrichment

**[openmbps-platform](https://github.com/montebelle/openmbps-platform)**: minimal framework for autonomous agent operations on a single machine.
Cron-driven, filesystem-backed, human-approved at publish gates. 28-cron orchestration with deterministic dedup keys, bounded retries, and explicit approval signals. Designed so a single operator can fan out across multiple client workspaces without losing the audit trail.

**[writings](https://github.com/montebelle/writings)**: technical essays on forecasting, causal inference, and agent system design.
