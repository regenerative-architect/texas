# Texas Master Systems OS v2.5.0 — Resilient AI PWA

This branch adds service-worker model staging, shard-level retry, offline folder model-pack import, a same-origin WebLLM runtime slot, optional local OpenAI-compatible endpoint fallback, and deterministic final fallback.

# v2.5.0 Resilient AI + Offline Model Packs

- Automatically detects browser-exposed WebGPU features/limits, shader-f16 support, CPU concurrency, browser-reported system-memory hints and origin storage.
- Builds a conservative hardware profile without claiming to measure exact GPU VRAM.
- Automatically selects the lowest-declared-VRAM compatible WebLLM model as the default.
- Adds Lightest / Balanced / Higher-capacity model recommendations based on the compatible catalog and hardware profile.
- Keeps manual model selection available and leaves larger recommendations opt-in.

# Texas Master Systems OS v2.5.0-resilient-ai

Experimental comparison branch of v2.2.2. The primary WebLLM path is now the direct `CreateMLCEngine` factory, with optional dedicated-worker fallback.

## Why this branch exists

The v2.2.2 compatibility build used the Connectivity v5 worker/IndexedDB pattern first. This edition intentionally reverses that choice so the same browser/network can be tested with the newer direct-engine factory path. If both architectures fail at the same host request, the problem is network/artifact reachability rather than the worker abstraction.

## AI changes

- Direct `CreateMLCEngine` first by default.
- Strategy selector: Direct → worker fallback, Direct only, or Worker only.
- Runtime import attempts `esm.run` first and a jsDelivr `+esm` URL second.
- Cache API is the default cache backend; IndexedDB and OPFS remain selectable.
- `Test model hosts` checks runtime CDN routes, the selected Hugging Face model config path, and the compiled WebGPU library route.
- Model catalog is grouped into collapsible Tiny / Light / Medium / Heavy / Extreme sections, with each model itself collapsible.
- Download/cache success is still verified with WebLLM cache inspection before being reported.

## Network reality

Engine switching can bypass a worker/module-runtime problem. It cannot bypass a firewall/content blocker that prevents the browser from reaching the model repository or compiled model library host. Host diagnostics are included specifically to distinguish those cases.

All other v2.2.2 systems, PWA, multiplayer, Nexus, Cascade Lab, evidence, quests, storage, and offline features are retained.


## v2.2.4 GPU feature-aware patch

This build fixes a diagnostic bug where an `https://` URL in a JavaScript stack trace could cause a WebGPU compatibility failure to be mislabeled as a network error. Error classification now uses the error message first and checks GPU compatibility before network conditions.

The model catalog now queries the browser WebGPU adapter, compares `required_features` and `buffer_size_required_bytes` against each WebLLM model record, defaults to **Compatible only**, disables unsupported models, and automatically selects the lowest-VRAM compatible model if the prior selection is incompatible. The readiness panel explicitly reports whether `shader-f16` is exposed.


## v2.5.0 Adaptive Planning Quality

This patch separates the lightest hardware-compatible startup model from the model recommended for a specific planning task. Complex prompts receive a deterministic systems scaffold, a larger output budget, a stronger planning system prompt, optional automatic switching to a stronger cached compatible model, and automatic retry when a local model produces a generic refusal or undersized answer.

The 360M fallback remains useful for compatibility testing and short transformations. For comprehensive synthesis, the app heuristically prefers >=1B parameter models when compatible; expert implementation work prefers >=2B when practical. These thresholds are heuristics, not benchmark guarantees.


## v2.5.0 — Meta-Chain Orchestrator

Adds Meta-Chain Studio as the orchestration layer above the adaptive/resilient WebLLM stack. Thirteen bundled chain families support guided, checkpointed autopilot and collaborative execution. Chain artifacts can attach Evidence Ledger entries, be shared through the existing local/P2P record layer, seed Nexus/Cascade workflows, and compile into projects, tasks, questions, handoffs, decisions and quests. Three new IndexedDB stores (`chains`, `chainRuns`, `chainArtifacts`) raise the backup schema to v5.


## v2.5.0 — Tiered Local AI + splash fail-safe

- Browser AI roles: 360M compatibility fallback, Llama 3.2 1B standard planning, Llama 3.2 3B advanced synthesis.
- New local-runtime layer for 8B+ models through an OpenAI-compatible localhost endpoint; LM Studio preset and model discovery are included.
- Remote endpoints remain explicit opt-in and API keys are not persisted.
- Generic Python and PowerShell model-pack builders can download any of the three supported browser tiers without committing model weights to normal git history.
- The splash now uses viewport/percentage positioning and an inline pre-app fail-safe. Enter, Skip, keyboard dismissal, progression dismissal, or the 7.2-second hard limit can close it even if application initialization fails.
- Service-worker registration requests fresh updates and the shell cache is bumped to v2.5.0.

See `docs/TIERED_LOCAL_AI.md`.
