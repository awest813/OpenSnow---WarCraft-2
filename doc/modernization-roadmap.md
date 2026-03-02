# Wargus Modernization & QoL Roadmap

## Current-state analysis (quick audit)

This roadmap is based on a lightweight audit of build, tooling, runtime scripts, and documentation.

### What is working well
- The project already uses CMake with C++17 enabled and has explicit dependency discovery for Stratagus/PNG/Zlib.
- There is active macOS CI and app bundle packaging.
- Gameplay content is mostly data-driven via Lua scripts (units, upgrades, menus, AI), which lowers cost for balancing/QoL iteration.

### High-impact pain points observed
1. **Platform and CI coverage is narrow**
   - GitHub Actions currently includes only a macOS workflow in this repository.
2. **Installer/extractor experience is still fragile**
   - The README explicitly says the built-in macOS extractor is not currently working and points users to a third-party script.
3. **Legacy native code risk in wartool area**
   - `wartool.cpp` is large and still contains manual memory patterns and older C APIs, raising maintenance risk.
4. **Build config drift signs**
   - Top-level CMake references `warextract.c`, but that source file is not present in the current tree.
5. **Lua gameplay scripts are powerful but hard to maintain**
   - Scripts are extensive and largely global/procedural, with limited guardrails for regression testing.

---

## Modernization roadmap

## Phase 0 (0-2 weeks): Stabilize the delivery pipeline

### Goals
- Make changes safer to ship.
- Prevent regressions from silently reaching users.

### Work items
1. **Add Linux + Windows CI workflows**
   - Build and smoke-test both `wargus` and `wartool`.
   - Cache dependencies where possible.
2. **Add script-level validation checks**
   - Add Lua syntax and load-order checks in CI (`luac -p` and minimal boot validation).
3. **Fix obvious build-config drift**
   - Remove or correct stale CMake source references (e.g., `warextract.c`) and fail fast on missing sources.
4. **Introduce release gates**
   - Require all platform workflows to pass before release tagging.

### QoL outcomes
- Faster turnaround on contributor PRs.
- Fewer "works on my machine" issues.

---

## Phase 1 (2-6 weeks): Improve onboarding and installation UX

### Goals
- Reduce first-run friction.
- Make extraction troubleshooting self-serve.

### Work items
1. **Repair and verify built-in macOS extraction flow**
   - Align launcher/extractor path assumptions with current app bundle layout.
2. **Add extraction diagnostics mode**
   - Single command/log bundle users can attach in bug reports.
3. **Refresh setup docs**
   - Replace fragmented install guidance with one cross-platform quick-start and a troubleshooting matrix.
4. **Add integrity checks for extracted assets**
   - Detect partial extraction and offer one-click repair.

### QoL outcomes
- Lower support burden for installation issues.
- Better confidence that users start from a healthy dataset.

---

## Phase 2 (1-2 quarters): Native code modernization and safety

### Goals
- Reduce crash surface and maintenance cost.
- Keep compatibility while modernizing internals.

### Work items
1. **Refactor `wartool.cpp` in slices**
   - Isolate archive I/O, image conversion, audio extraction into smaller modules.
2. **Replace manual memory ownership patterns**
   - Move from raw pointers/arrays to RAII containers (`std::vector`, smart pointers, `std::string`).
3. **Add sanitizers and static analysis in CI**
   - AddressSanitizer/UBSan builds, plus `clang-tidy` pass on touched native files.
4. **Add focused regression tests**
   - Golden-file tests for representative extraction outputs.

### QoL outcomes
- Fewer extraction crashes and data-corruption edge cases.
- Safer iteration speed for future features.

---

## Phase 3 (parallel track): Gameplay and UI QoL backlog

### Shortlist (player-visible wins)
1. **Input/UI accessibility improvements**
   - Better default keybindings, larger UI presets, optional high-contrast overlays.
2. **Campaign usability improvements**
   - Mid-mission retry prompts, optional autosave checkpoints, clearer failure causes.
3. **Multiplayer lobby polish**
   - Better status visibility, map/mod mismatch messaging, ping/latency hints.
4. **In-game discoverability**
   - Contextual tooltips for advanced mechanics and hotkeys.

### Implementation note
- Prefer shipping this as **small monthly QoL bundles** with explicit changelog labels (`QoL`, `Accessibility`, `MP`).

---

## Execution model and prioritization

### Recommended order
1. Phase 0 (pipeline confidence)
2. Phase 1 (installation UX)
3. Phase 2 (native reliability)
4. Phase 3 continuously in parallel as bandwidth allows

### Suggested ownership
- **Build/CI owner**: pipelines, release gates, dependency caching
- **Runtime owner**: extraction + launcher UX
- **Gameplay owner**: Lua refactors + QoL backlog curation

### Success metrics
- CI pass rate by platform
- Installation success rate (first run)
- Crash reports related to extraction per release
- QoL issue closure rate and time-to-fix

---

## First 10 concrete tickets to open

1. Add `linux.yml` GitHub Actions workflow for configure/build/test.
2. Add `windows.yml` GitHub Actions workflow for configure/build.
3. Add Lua syntax validation job (`luac -p`) over `scripts/**/*.lua`.
4. Fix/remove stale `warextract.c` CMake source definition.
5. Add `--diagnostics` output mode to launcher/extractor path.
6. Add extraction integrity check command and UI hook.
7. Split `wartool.cpp` PNG writing code into dedicated translation unit.
8. Introduce ASan/UBSan CI preset for native tools.
9. Add golden extraction test fixture for one sample archive.
10. Publish `doc/quick-start.md` with platform troubleshooting table.
