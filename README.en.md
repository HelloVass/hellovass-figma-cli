# figma-cli

[中文](README.md) | [English](README.en.md)

This is the public distribution repository for `figma-cli`.

This repository publishes the self-contained fat jar, SHA-256 checksum, and CLI usage guide. `figma-cli` is a
deterministic Figma REST export CLI: it reads raw design nodes, converts them into a platform-agnostic ViewTree IR,
and writes screenshots, assets, and other verifiable evidence to disk.

## Current Version

- Version: `0.1.4`
- Jar: [`figma-cli-0.1.4-all.jar`](https://github.com/HelloVass/hellovass-figma-cli/releases/download/v0.1.4/figma-cli-0.1.4-all.jar)
- Checksum: [`figma-cli-0.1.4-all.jar.sha256`](https://github.com/HelloVass/hellovass-figma-cli/releases/download/v0.1.4/figma-cli-0.1.4-all.jar.sha256)
- SHA-256: `bb7eb296d12cfbf1553801338fe1700823c3a82820ef9305163a3b01a16e22ea`

## Requirements

- Java 17 or later.
- A Figma Personal Access Token with `File content read` and `File images read` permissions.

## Installation

Download the jar and checksum of the current version, then verify integrity:

```bash
curl -fLO https://github.com/HelloVass/hellovass-figma-cli/releases/download/v0.1.4/figma-cli-0.1.4-all.jar
curl -fLO https://github.com/HelloVass/hellovass-figma-cli/releases/download/v0.1.4/figma-cli-0.1.4-all.jar.sha256
shasum -a 256 -c figma-cli-0.1.4-all.jar.sha256
```

The token is read only from the `FIGMA_TOKEN` environment variable, never from command-line arguments:

```bash
export FIGMA_TOKEN="figd_xxx"
```

## Quick Start

```bash
# Top-level and subcommand --help output is the single source of truth for arguments, defaults, and output contracts
java -jar figma-cli-0.1.4-all.jar --help

# Check the local environment before touching Figma; runs offline but verifies FIGMA_TOKEN is set
java -jar figma-cli-0.1.4-all.jar doctor

# Export raw JSON, ViewTree IR, screenshot, screenshot metadata, asset manifest, and assets in one run
java -jar figma-cli-0.1.4-all.jar pull "https://www.figma.com/design/<file-key>/<file-name>?node-id=<node-id>" --output <case-dir>
```

## Commands

| Command | Purpose |
|---------|---------|
| `tree` | Fetch the ViewTree IR of the target node. |
| `raw` | Fetch the raw JSON of the Figma nodes endpoint. |
| `frames` | List pages and top-level frames/components in a file. |
| `annotations` | Preserve native REST annotations, match canvas-side annotations, and emit a per-target coverage report. |
| `screenshot` | Write the target node screenshot to disk and print metadata JSON with the output path to stdout. |
| `assets` | Collect and download assets declared in an exported ViewTree IR. |
| `pull` | Produce raw, IR, screenshot, screenshot metadata, asset manifest, and asset directory in one run. |
| `doctor` | Check the local environment before accessing Figma. |
| `skills` | List, find, locate, add, replace, or remove trusted built-in Skills shipped inside the CLI. |

Read the `--help` of a command before calling it; command names, arguments, defaults, and output contracts follow the help output.

## Boundaries

`figma-cli` does not modify Figma files and does not generate Compose / SwiftUI / Flutter / React Native code.
It outputs a platform-agnostic ViewTree IR and evidence; platform implementation is done by agents or downstream projects.
Source code and development live in a private repository; this repository only hosts distribution artifacts and docs.
