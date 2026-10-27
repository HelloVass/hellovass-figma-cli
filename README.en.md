# figma-cli

[中文](README.md) | [English](README.en.md)

This is the public distribution repository for `figma-cli`.

This repository publishes native single-file binaries, SHA-256 checksums, and the CLI usage guide. `figma-cli`
is a deterministic Figma REST export CLI: it reads raw design nodes, converts them into a platform-agnostic
ViewTree IR, and writes screenshots, assets, and other verifiable evidence to disk. The binaries are compiled
with GraalVM native-image and **require no Java runtime**.

## Current Version

- Version: `0.1.5`
- darwin-arm64 (macOS Apple Silicon): [`figma-cli-darwin-arm64`](https://github.com/HelloVass/hellovass-figma-cli/releases/download/v0.1.5/figma-cli-darwin-arm64) (SHA-256: `3a6986c384dbd0566ca06404c80d39aff117cf314e0d9b636dd98552d2716206`)
- linux-x64: [`figma-cli-linux-x64`](https://github.com/HelloVass/hellovass-figma-cli/releases/download/v0.1.5/figma-cli-linux-x64) (SHA-256: `79565b7913856fc0689fa3bfb4ad3591e33d9779f711efc5a2beb622ea142c9c`)

## Installation

Download the binary for your platform and verify it (macOS Apple Silicon shown):

```bash
curl -fLO https://github.com/HelloVass/hellovass-figma-cli/releases/download/v0.1.5/figma-cli-darwin-arm64
curl -fLO https://github.com/HelloVass/hellovass-figma-cli/releases/download/v0.1.5/figma-cli-darwin-arm64.sha256
shasum -a 256 -c figma-cli-darwin-arm64.sha256
chmod +x figma-cli-darwin-arm64
```

Optionally put it on your PATH:

```bash
mkdir -p ~/.local/bin && mv figma-cli-darwin-arm64 ~/.local/bin/figma-cli
```

On first run macOS Gatekeeper may block the unsigned binary; clear the quarantine flag:

```bash
xattr -d com.apple.quarantine ~/.local/bin/figma-cli
```

The token is read only from the `FIGMA_TOKEN` environment variable, never from command-line arguments:

```bash
export FIGMA_TOKEN="figd_xxx"
```

## Quick Start

```bash
# Top-level and subcommand --help output is the single source of truth for arguments, defaults, and output contracts
figma-cli --help

# Check the local environment before touching Figma; runs offline but verifies FIGMA_TOKEN is set
figma-cli doctor

# Export raw JSON, ViewTree IR, screenshot, screenshot metadata, asset manifest, and assets in one run
figma-cli pull "https://www.figma.com/design/<file-key>/<file-name>?node-id=<node-id>" --output <case-dir>
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
