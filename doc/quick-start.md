# Wargus Quick-Start Guide

This guide covers installing and running Wargus on all supported platforms, plus a troubleshooting reference.

---

## Prerequisites

Wargus is a mod for the [Stratagus](https://github.com/Wargus/stratagus) engine.
You need:

1. **The Stratagus engine** – download a release binary or build from source.
2. **Warcraft 2 game data** – you must own a legal copy. Supported sources:
   - Warcraft 2 DOS CD-ROM (REZDAT.WAR / STRDAT.WAR / MAINDAT.WAR)
   - Warcraft 2 BNE (Battle.net Edition) – `INSTALL.MPQ` / `INSTALL.EXE`
   - GoG.com installer `.exe`

---

## Platform quick-start

### Linux

#### Install from package manager (where available)

Some distributions package Wargus directly:

```bash
# Debian / Ubuntu
sudo apt-get install wargus
```

#### Build from source

```bash
# Install build dependencies
sudo apt-get install cmake libsdl2-dev libsdl2-mixer-dev libsdl2-image-dev \
  liblua5.4-dev libpng-dev zlib1g-dev libbz2-dev libx11-dev libgtk2.0-dev ffmpeg

# Clone and build
git clone --recurse-submodules https://github.com/Wargus/wargus.git
cd wargus
cmake -B build -DCMAKE_BUILD_TYPE=Release \
  -DSTRATAGUS_INCLUDE_DIR=/path/to/stratagus/gameheaders \
  -DSTRATAGUS=/path/to/stratagus/binary
cmake --build build -- -j$(nproc)
```

#### Extract game data

```bash
# Typical invocation (DOS CD-ROM)
./build/wartool /path/to/warcraft2-cd /path/to/output-data

# With video conversion and CD music ripping
./build/wartool -v -r /path/to/warcraft2-cd /path/to/output-data
```

#### Launch

```bash
stratagus -d /path/to/output-data
```

---

### Windows

#### Download pre-built binaries

Download the latest release from the
[Wargus releases page](https://github.com/Wargus/wargus/releases) and run the
NSIS installer. It will place `wargus.exe` and `wartool.exe` in the chosen
installation directory.

#### Extract game data

Open a command prompt and run:

```cmd
wartool.exe C:\path\to\warcraft2-cd C:\path\to\output-data
```

A log file is automatically written to:

```
%APPDATA%\Stratagus\wartool.txt
```

#### Launch

Double-click `wargus.exe` or run it from the command prompt.

---

### macOS

> **Note:** The built-in macOS extractor is currently undergoing repair. If it
> does not work for you, use the
> [third-party extraction script](https://github.com/shinra-electric/Stratagus-Data-Extractor-Script)
> as a temporary workaround.

#### Download pre-built app bundle

Download the latest `.dmg` artifact from
[GitHub Actions → macOS workflow](https://github.com/Wargus/wargus/actions/workflows/macos.yml),
mount it, and drag `Wargus.app` to your Applications folder.

#### Extract game data (Terminal)

```bash
# Inside the app bundle
/Applications/Wargus.app/Contents/MacOS/wartool /path/to/warcraft2-cd \
  ~/Library/Application\ Support/Wargus/data
```

#### Launch

Open `Wargus.app` from Finder or Launchpad.

---

## Data extraction reference

```
wartool [-e|-n] [-v] [-r] [-V] [-h] <archive-dir> [<dest-dir>]
        --check       <dest-dir>
        --diagnostics <dest-dir>
```

| Flag | Meaning |
|------|---------|
| `-e` | Force expansion-CD mode |
| `-n` | Force non-expansion mode |
| `-v` | Extract and convert video cutscenes |
| `-r` | Rip music from CD-ROM (requires physical disc) |
| `-V` | Print tool version |
| `-h` | Print usage |
| `--check` | Verify integrity of already-extracted assets |
| `--diagnostics` | Print a full diagnostic report for a bug report |

### Checking integrity after extraction

```bash
# Linux / macOS
./wartool --check /path/to/output-data

# Windows
wartool.exe --check C:\path\to\output-data
```

Exit code is `0` if all required assets are present, non-zero if required files
are missing (re-run extraction in that case).

### Generating a diagnostics bundle for bug reports

```bash
./wartool --diagnostics /path/to/output-data 2>&1 | tee wargus-diag.txt
```

Attach `wargus-diag.txt` when filing a bug report.

---

## Troubleshooting matrix

| Symptom | Likely cause | Fix |
|---------|-------------|-----|
| `wartool` exits immediately with no output | Archive directory not found or wrong path | Check that the path contains `REZDAT.WAR`, `MAINDAT.WAR`, or `INSTALL.MPQ` |
| `Cannot open REZDAT.WAR` | Wrong archive directory or CD not mounted | Mount the CD or verify the path; try both upper- and lower-case file names |
| Game starts but shows no graphics | Extraction incomplete | Run `wartool --check <dest>` and re-extract if files are missing |
| `wc2-config.lua` not found on startup | Extraction did not complete | Re-run wartool; check that `<dest>/scripts/wc2-config.lua` exists |
| Music does not play | CD music not ripped, or converter missing | Re-run with `-r` flag; ensure `ffmpeg` is installed |
| Video cutscenes missing | Extraction run without `-v` | Re-run with `-v` flag |
| macOS extractor produces errors | Known issue with app-bundle path assumptions | Use the [third-party extraction script](https://github.com/shinra-electric/Stratagus-Data-Extractor-Script) |
| Windows: log file location | Unsure where to find the log | Check `%APPDATA%\Stratagus\wartool.txt` |
| Build error: `warextract.c not found` | Stale CMake cache from an old checkout | Delete your build directory and re-run CMake |

---

## Useful links

- [Wargus GitHub](https://github.com/Wargus/wargus)
- [Stratagus engine GitHub](https://github.com/Wargus/stratagus)
- [stratagus.com](https://stratagus.com)
- [Community Discord](https://discord.gg/dQGxaw3QfB)
- [Community Gitter](https://gitter.im/Wargus)
