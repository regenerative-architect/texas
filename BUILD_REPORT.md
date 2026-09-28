# Texas Master Systems OS v2.5.0 — Tiered Local AI build report

## Delivered changes

- Added a five-layer AI execution stack: 360M browser fallback, Llama 3.2 1B standard browser planning, Llama 3.2 3B advanced browser synthesis, OpenAI-compatible localhost runtime for 8B+ models, and explicit opt-in remote endpoint fallback.
- Added a compact Tiered Local AI module with model selection/staging controls, routing policy, LM Studio localhost preset, `/v1/models` discovery, and same-origin companion-server preset.
- Kept hardware autodetection and **lightest compatible** automatic startup selection. The standard 1B and advanced 3B tiers are quality upgrades rather than surprise downloads.
- Added `tools/download_model_pack.py` and `tools/download_model_pack.ps1` for the three browser tiers so weights do not have to live in ordinary git history.
- Updated the optional GitHub Pages workflow so deployment can choose no pack, fallback 360M, standard 1B, or advanced 3B.
- Added `local-runtime/serve_local.py`: serves the complete PWA on `127.0.0.1:8765` and proxies `/local-ai/v1` to a local OpenAI-compatible runtime (default `127.0.0.1:1234/v1`). This removes the hosted-origin → localhost browser boundary when needed.
- Preserved PWA shard staging, offline folder model packs, same-origin hosted packs, WebLLM direct/worker fallbacks, adaptive planning, deterministic fallback, Meta-Chain orchestration, Trystero/WebRTC collaboration, and all ten Texas systems.

## Splash fix

The splash is no longer dependent on successful application boot. `index.html` installs an inline pre-app fail-safe immediately after the splash markup. Enter, Skip, Enter/Space/Escape, normal progress completion, or a 7.2-second hard timeout can dismiss the overlay even when IndexedDB or another application subsystem throws before `app.js` initializes.

The Texas scene and splash layout now use percentage/viewport geometry (`%`, `vw`, `vh`, `clamp()`, aspect ratios) for placement and scale rather than desktop-specific pixel coordinates. The main scene is centered with percentage transforms and the card is viewport-bounded and scrollable on unusually short screens.

## Automated checks performed

- `node --check app.js` — PASS
- `node --check ai-tiered-stack.js` — PASS
- `node --check meta-chain.js` — PASS
- `node --check ai-resilience.js` — PASS
- `node --check webllm-worker.js` — PASS
- `node --check sw.js` — PASS
- Python compilation for model-pack and local companion scripts — PASS
- Existing structural verifier — PASS
- v2.5 tier/local-runtime/splash markers — PASS
- Runtime unit test for AI-tier initialization + independent splash Enter dismissal + hard fail-safe timer — PASS
- Service-worker CORE asset existence check: 19 entries, 0 missing — PASS
- Local companion HTTP smoke: `/`, `index.html`, `app.js`, `ai-tiered-stack.js`, `sw.js`, model-pack index all returned HTTP 200 — PASS
- Local companion proxy correctly returned HTTP 502 when no upstream model server was running, demonstrating explicit failure rather than silent hanging — PASS
- Generic model-pack installer CLI exposes all three supported browser model IDs — PASS

## Deployment-stage tests still required

This environment cannot supply the user's GPU or a live LM Studio/Ollama-style server. The following should be tested on the target machine: real WebGPU model generation, actual 1B/3B model staging, real localhost 8B+ inference, and live two-device WebRTC/TURN collaboration.
