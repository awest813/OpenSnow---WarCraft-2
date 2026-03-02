# Wargus

<img src="./ico_alt.svg" width="180" align="right" />

Wargus is an open-source Warcraft II data mod for the Stratagus engine. It preserves the classic Warcraft II look-and-feel while adding modern quality-of-life improvements and cross-platform packaging.

## What you need

Wargus does **not** ship Warcraft II assets. You must legally own Warcraft II data files and extract them before playing.

## Quick start

1. Download a Wargus build for your platform.
2. Extract Warcraft II game data into the Wargus data directory.
3. Launch `wargus`.

For a step-by-step setup guide, see [`doc/quick-start.md`](doc/quick-start.md).

## macOS extraction status

The built-in extractor on macOS is still being stabilized.

If extraction fails, use the community script:
- <https://github.com/shinra-electric/Stratagus-Data-Extractor-Script>

Run it in the same folder as your Warcraft II installer files and the Wargus app bundle.

## Build from source

Wargus uses CMake.

```bash
cmake -S . -B build
cmake --build build
```

Binary targets include:
- `wargus` (game executable)
- `wartool` (asset/extraction utility)

## Project docs

- Quick start: [`doc/quick-start.md`](doc/quick-start.md)
- Modernization plan: [`doc/modernization-roadmap.md`](doc/modernization-roadmap.md)
- PUD format notes: [`doc/pud-specs.txt`](doc/pud-specs.txt)
- Changelog: [`doc/changelog`](doc/changelog)

## Community

- Discord: <https://discord.gg/dQGxaw3QfB>
- Gitter: <https://gitter.im/Wargus>
- Engine translation strings: <https://poeditor.com/join/project?hash=BVo8MAZuys>
- Game translation strings: <https://poeditor.com/join/project?hash=7myZPTmcqq>

## License

- Project license: GPL (see [`COPYING`](COPYING))
- Third-party notices: [`COPYING-3rd`](COPYING-3rd)
