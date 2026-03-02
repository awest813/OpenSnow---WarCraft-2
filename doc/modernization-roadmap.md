# Wargus Modernization Roadmap (2026)

This roadmap reframes modernization work into practical milestones focused on stability first, then developer velocity, then player-facing improvements.

---

## Strategic goals

1. **Reliable releases across platforms**
2. **Low-friction installation and extraction**
3. **Safer native code and easier maintenance**
4. **Continuous quality-of-life (QoL) delivery**

---

## Phase 0 (Weeks 1-2): Build and release reliability

### Objectives
- Catch regressions earlier.
- Ensure every tagged release is reproducible.

### Deliverables
- Add CI coverage for Linux and Windows in GitHub Actions.
- Build both `wargus` and `wartool` in each platform workflow.
- Add fast script validation (`luac -p`) over the Lua script set.
- Remove stale build references and fail on missing source entries.
- Define release gate rules: all required workflows must pass before tagging.

### Exit criteria
- CI green on all supported platforms for 5 consecutive merges.
- No known broken source references in CMake configuration.

---

## Phase 1 (Weeks 3-6): Installation and extraction UX

### Objectives
- Reduce first-run failures.
- Make troubleshooting easy for players and maintainers.

### Deliverables
- Stabilize built-in macOS extraction path behavior.
- Add extraction diagnostics output users can attach to bug reports.
- Publish a single canonical setup flow in `doc/quick-start.md`.
- Add extraction integrity checks with clear remediation guidance.

### Exit criteria
- New users can complete setup from docs without maintainer intervention.
- Most extraction bug reports include actionable diagnostics data.

---

## Phase 2 (Quarterly track): Native code safety and maintainability

### Objectives
- Reduce crash risk in extraction/tooling paths.
- Make core native code easier to refactor.

### Deliverables
- Split `wartool.cpp` into focused modules (archive I/O, image conversion, audio extraction).
- Replace manual ownership patterns with RAII containers and smart pointers.
- Add sanitizer builds (ASan/UBSan) for CI validation on native targets.
- Introduce targeted golden-file tests for representative extraction outputs.

### Exit criteria
- Sanitizer jobs are stable and actionable.
- Critical extraction code has regression coverage.

---

## Phase 3 (Continuous): Player-facing QoL cadence

### Objectives
- Ship incremental improvements regularly.
- Keep changes easy to verify and communicate.

### Candidate workstreams
- Accessibility presets (UI scale, contrast, keybinding defaults).
- Campaign usability improvements (retry prompts, autosave checkpoints).
- Multiplayer lobby clarity (version/map mismatch messaging, latency hints).
- In-game discoverability (advanced mechanic and hotkey tooltips).

### Delivery model
- Monthly QoL bundles with changelog tags: `QoL`, `Accessibility`, `Multiplayer`, `UX`.

---

## Ownership model

- **Build & release owner**: CI, packaging, release gates.
- **Runtime/tooling owner**: extraction pipeline, diagnostics, setup flow.
- **Gameplay/UI owner**: Lua content evolution and QoL backlog.

---

## Suggested KPI dashboard

- Platform CI pass rate by branch.
- Mean time to repair broken builds.
- First-run extraction success rate.
- Extraction-related crashes per release.
- QoL issue lead time (opened → shipped).

---

## First 12 issues to open

1. Add `linux.yml` GitHub Actions workflow (configure/build/test).
2. Add `windows.yml` GitHub Actions workflow (configure/build).
3. Add Lua syntax validation job for `scripts/*.lua`.
4. Audit and remove stale CMake source references.
5. Add `--diagnostics` mode for extraction and startup logging.
6. Add extraction integrity checker with clear user messaging.
7. Move PNG export logic out of `wartool.cpp` into a dedicated module.
8. Move audio extraction logic out of `wartool.cpp` into a dedicated module.
9. Add ASan CI preset for supported compiler targets.
10. Add UBSan CI preset for supported compiler targets.
11. Add one golden extraction fixture with deterministic output checks.
12. Refresh `doc/quick-start.md` with troubleshooting matrix and expected paths.
