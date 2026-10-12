# TODO — TPT Offline AI Media Studio

Tick boxes as you go. Decisions locked from planning are listed first.

## Decisions (locked)
- Foundation crates + Keystone: pulled from GitHub as git dependencies (exact repo URLs/revs to be confirmed in 0.1)
- UI: pure tpt-appfront native (no Tauri)
- Speech models: bundle one small default model AND allow user-provided models (GGUF etc.)
- Timeline: multi-track (2–4+ tracks) with cut/trim/basic transitions
- Project storage: Keystone-backed
- License key: not enforced (informational only; OSS builds identical in function)
- Formats: whatever kinetix/cadence support; gaps are documented, not patched with FFmpeg
- Licence: MIT OR Apache-2.0; all deps must be compatible with both

## Phase 0 — Setup & Discovery
- [x] 0.1 Locate repos (all under https://github.com/tpt-solutions/, default branch `master`): tpt-kinetix, tpt-cadence, tpt-visual, tpt-voice, tpt-gpu, tpt-appfront, tpt-simd, tpt-crypto, tpt-primitives, tpt-keystone-db (Keystone)
- [ ] 0.1b Pin each dependency to a specific commit SHA (branches are moving; repos pushed daily)
- [ ] 0.1c Verify real licences: GitHub shows Apache-2.0 only (cadence, visual, voice, appfront, simd, crypto, primitives) or "Other" (kinetix, gpu, keystone-db) even where READMEs claim MIT OR Apache-2.0. Check LICENSE files; ask upstream to add LICENSE-MIT where missing. Blocks the dual-licence rule.
- [ ] 0.2 Create Cargo workspace (`app`, `core`, `project`, `ui`, etc.) and pin git revs
- [ ] 0.3 Add LICENSE-MIT, LICENSE-APACHE, README, CONTRIBUTING
- [ ] 0.4 Add `cargo-deny` config to enforce MIT/Apache-compatible deps (no Apache-only)
- [ ] 0.5 CI for Windows, macOS, Linux (build, test, clippy, fmt, deny)
- [ ] 0.6 Audit each foundation crate: public API, supported codecs/containers, maturity, gaps → `docs/crate-audit.md`
- [ ] 0.7 Audit Keystone API (schema, transactions, file layout, encryption support)
- [ ] 0.8 Produce supported-formats list (feeds import/export scope)
- [ ] 0.9 Add CLAUDE.md / dev notes (optional)

## Phase 1 — Architecture
- [ ] 1.1 Write architecture doc: module boundaries, threading, data flow
- [ ] 1.2 Define project data model (assets, tracks, clips, effects, transcripts, settings)
- [ ] 1.3 Define Keystone schema + versioning/migration strategy
- [ ] 1.4 Define job/pipeline model (shared by editor export and batch)
- [ ] 1.5 Define effect-stack model (CPU/GPU paths, fallback)
- [ ] 1.6 Define error handling, logging (local only), crash-report bundle format
- [ ] 1.7 Resolve open question: binary size targets and per-platform budgets

## Phase 2 — Core Media Layer
- [ ] 2.1 Media probe (container, streams, duration, metadata)
- [ ] 2.2 Video decode via kinetix (frame-accurate seek)
- [ ] 2.3 Audio decode via cadence
- [ ] 2.4 Proxy/thumbnail/waveform generation + cache
- [ ] 2.5 Import pipeline with unsupported-format error UX
- [ ] 2.6 Export/encode pipeline (video + audio mux)
- [ ] 2.7 Export presets
- [ ] 2.8 Hardware encoder detection/use where crates support it
- [ ] 2.9 Benchmarks vs FFmpeg on reference clips (success criterion)

## Phase 3 — Project System
- [ ] 3.1 Project create/open/save/autosave on Keystone
- [ ] 3.2 Media references (relative paths, relink missing media)
- [ ] 3.3 Undo/redo command history
- [ ] 3.4 Project package export (media + project, zip-like)
- [ ] 3.5 Optional package encryption on export (tpt-crypto) + import/decrypt
- [ ] 3.6 Schema migration tests
- [ ] 3.7 Recent projects list

## Phase 4 — Timeline Editing (multi-track)
- [ ] 4.1 Timeline model: tracks (video/audio), clips, ordering, snapping
- [ ] 4.2 Cut / split, trim, move, delete, ripple
- [ ] 4.3 Multiple video and audio tracks, mute/solo/lock
- [ ] 4.4 Basic transitions (cut, crossfade/dissolve, fade in/out)
- [ ] 4.5 Playback engine with A/V sync, scrubbing
- [ ] 4.6 Real-time preview (with proxy fallback)
- [ ] 4.7 Timeline renderer → export
- [ ] 4.8 Keyboard shortcuts

## Phase 5 — Audio Tools
- [ ] 5.1 Loudness normalisation (LUFS/peak)
- [ ] 5.2 Voice isolation / noise reduction
- [ ] 5.3 Basic effects (EQ, gain, fades, compressor — scope per cadence)
- [ ] 5.4 Per-clip and per-track audio effect stacks
- [ ] 5.5 Meters + waveform display

## Phase 6 — Speech (tpt-voice)
- [ ] 6.1 Model manager: bundled default, import user model, validate, list, delete
- [ ] 6.2 Choose and license-check the bundled model (must be redistributable)
- [ ] 6.3 Transcription with word-level timestamps
- [ ] 6.4 Diarization (speaker labels) if supported
- [ ] 6.5 Transcript store in project
- [ ] 6.6 Text-based editing: delete words → cuts audio/video on timeline
- [ ] 6.7 Export subtitles (SRT/VTT) and transcript text
- [ ] 6.8 GPU acceleration for inference via tpt-gpu, CPU fallback
- [ ] 6.9 Background job UI with progress/cancel

## Phase 7 — Visual Effects (tpt-visual / tpt-gpu)
- [ ] 7.1 GPU capability detection + CPU fallback policy
- [ ] 7.2 Basic colour adjustments (brightness, contrast, saturation, etc.)
- [ ] 7.3 Basic effects set (blur, crop, scale, transform, opacity)
- [ ] 7.4 Effect stack UI + parameters + preview
- [ ] 7.5 SIMD kernels (tpt-simd) for CPU paths
- [ ] 7.6 Effects applied identically in preview and export

## Phase 8 — Batch Processing
- [ ] 8.1 Batch queue model (inputs, preset, output rules)
- [ ] 8.2 Queue UI: add folder/files, pause, cancel, retry
- [ ] 8.3 Parallelism and resource limits
- [ ] 8.4 Per-job logs and summary report

## Phase 9 — UI (tpt-appfront)
- [ ] 9.1 App shell, window, menus, theming (dark default)
- [ ] 9.2 Media bin / import panel
- [ ] 9.3 Preview viewer
- [ ] 9.4 Timeline widget
- [ ] 9.5 Inspector (clip/effect properties)
- [ ] 9.6 Transcript panel
- [ ] 9.7 Export dialog + batch panel
- [ ] 9.8 Settings (cache, models, GPU, paths)
- [ ] 9.9 Gaps found in appfront → file issues / contribute upstream
- [ ] 9.10 Accessibility and HiDPI pass
- [ ] 9.11 Coherent look/feel review ("one professional tool")

## Phase 10 — Privacy & Security
- [ ] 10.1 Verify no network code paths (dependency audit, runtime test with network blocked)
- [ ] 10.2 No telemetry/accounts; document in README
- [ ] 10.3 Crash/error report as local file the user chooses to send
- [ ] 10.4 Optional license-key file read (informational only, no gating)
- [ ] 10.5 Fuzz/robustness tests on media import

## Phase 11 — Testing & Quality
- [ ] 11.1 Unit tests per module
- [ ] 11.2 Golden-file tests for export correctness
- [ ] 11.3 End-to-end offline scenario: import → edit → transcribe → export with network disabled
- [ ] 11.4 Cross-platform manual test matrix (Win/macOS/Linux)
- [ ] 11.5 Performance and memory profiling on large projects
- [ ] 11.6 `unsafe` audit and policy

## Phase 12 — Packaging & Distribution
- [ ] 12.1 Release builds for Windows, macOS, Linux
- [ ] 12.2 Installers (MSI/EXE, DMG/PKG, AppImage/deb)
- [ ] 12.3 Code signing (Windows cert, Apple notarisation)
- [ ] 12.4 Bundle default speech model; measure size
- [ ] 12.5 Build reproducibility notes for OSS builders
- [ ] 12.6 Example projects and presets
- [ ] 12.7 Quick-start guide (PDF)
- [ ] 12.8 License key file generation process for Gumroad
- [ ] 12.9 Assemble Gumroad zip (binaries + key + docs + examples)
- [ ] 12.10 Finalise GUMROAD.md (check claims against what's actually built, e.g. 3 GB disk)
- [ ] 12.11 Error-report intake process (email/form) documented

## Phase 13 — Release
- [ ] 13.1 Publish source repo (MIT OR Apache-2.0)
- [ ] 13.2 Tag v1.0
- [ ] 13.3 Publish Gumroad listing, test purchase flow
- [ ] 13.4 Verify success criteria (offline workflow, perf vs FFmpeg, coherence, shared codebase)

## Later (no commitment)
- [ ] Advanced colour grading and compositing
- [ ] Stronger on-device AI models
- [ ] Scriptable batch pipelines / CLI mode
- [ ] Hardware encoder integration improvements

## Assumptions (confirmed) and risks found on GitHub
- Multi-track depth: 2–4 tracks each of video/audio for MVP (confirmed)
- Keystone = `tpt-keystone-db`, a large multi-engine data platform (relational/graph/vector/etc.), licence "Other". Check it can be embedded in-process and is not oversized for a desktop app; fall back to file-based storage if audit (0.7) says no
- tpt-simd describes itself as "Design phase" — may be incomplete; kinetix/visual/voice may use it or have own kernels (check in 0.6)
- tpt-gpu includes a GGUF LLM inference engine — likely the route for tpt-voice models; confirm in 0.6
- tpt-appfront targets canvas/DOM/terminal/webview; confirm a true native desktop target with video-preview and custom timeline widgets exists
- Several upstream licence fields conflict with README claims (see 0.1c)
