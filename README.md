# Tachyon plugin

[![Tachyon 1.2.0](https://img.shields.io/github/v/release/formray/tachyon?label=Tachyon)](https://github.com/formray/tachyon/releases/latest)
[![MCP Registry: io.github.formray/tachyon](https://img.shields.io/badge/MCP%20Registry-io.github.formray%2Ftachyon-6f42c1.svg)](https://registry.modelcontextprotocol.io/v0/servers?search=io.github.formray/tachyon)
[![License: MIT OR Apache-2.0](https://img.shields.io/badge/license-MIT%20OR%20Apache--2.0-blue.svg)](#licence)
[![Windows 10/11](https://img.shields.io/badge/Windows-10%20%7C%2011-0078d4.svg)](#requirements)
[![Read-only](https://img.shields.io/badge/tools-read--only-2ea44f.svg)](#what-it-runs-sends-and-stores)

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/logo-dark.svg">
  <img alt="Tachyon, by Formray" src="assets/logo-light.svg" width="420">
</picture>

> Lets an AI agent read **this Windows PC's NVIDIA GPU** through
> [Tachyon](https://github.com/formray/tachyon), a lightweight, read-only GPU monitor.
> One plugin for Claude Code, Codex and Grok Build.

By [Formray](https://formray.io).

Ask things like *"why is my render slow?"*, *"will this model fit in VRAM?"*, *"why did
my PC switch off?"*, *"will this 4K render fit?"*, *"who was using VRAM at 18:20?"* or
*"was this render faster than last time?"*. The
agent answers from Tachyon's live readings, its flight recorder and its job history.

## What it contains

| Path | What |
|---|---|
| `skills/tachyon-gpu/SKILL.md` | when to use each tool and how to read the answers (Agent Skills format) |
| `.mcp.json`, `mcp.json` | start Tachyon's local MCP server: `tachyon mcp` over stdio |
| `.claude-plugin/plugin.json`, `plugin.json` | plugin manifests (Claude Code and Grok Build; Agent Plugins) |
| `.claude-plugin/marketplace.json` | lets Claude Code add this repo as a one-plugin marketplace |

No executables, scripts or hooks. Tachyon itself is installed separately.

## Requirements

- Windows 10 or 11, 64-bit, with an NVIDIA GPU and its driver.
- **Tachyon 1.2.0 or later**, with `tachyon` on PATH (with 1.1.0, `get_job_reports`,
  `get_process_vram` and `get_pcie_link` are missing; with 1.0.1 also `diagnose_crash` and
  `preflight`). Download `Tachyon_v1.2.0.zip` from
  the [releases](https://github.com/formray/tachyon/releases/latest), unzip it to a folder
  of your own (for example `C:\Tools\Tachyon`) and add that folder to your user PATH.
  winget (`Formray.Tachyon`) and Scoop packages are prepared and will put it on PATH for
  you once they are listed.
- In Tachyon: right-click → **Settings… → AI agents → Agent API** on. Until then every
  tool answers that the agent API is off. Keep Tachyon running for live data.

## Install

**Grok Build:** install *tachyon* from the plugin marketplace, or add this repo as a plugin
source.

**Claude Code:**

```bash
claude plugin marketplace add formray/tachyon-plugin
claude plugin install tachyon@formray
# or only the MCP server:
claude mcp add tachyon -- tachyon mcp
```

**Codex (CLI, IDE, ChatGPT desktop):**

```bash
codex mcp add tachyon -- tachyon mcp
```

then copy `skills/tachyon-gpu` into `~/.codex/skills/`.

**Claude Desktop** uses the one-click extension instead: download
`tachyon-1.2.0-windows-x64.mcpb` from the
[Tachyon release](https://github.com/formray/tachyon/releases/latest) and open it
(SHA-256 `f37a87a4200f4cd33f0caf8e641ba64d87cf3d958c923d4493245257143bb59e`, also in the
release's `SHA256SUMS.txt`).

Tachyon's MCP server is also listed in the official
[MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=io.github.formray/tachyon)
as `io.github.formray/tachyon`.

## What it runs, sends and stores

- Runs one local program, `tachyon mcp`, which talks to the agent over stdin and stdout.
  It listens on no port.
- **No network access**, no telemetry, no downloads, no credentials. Nothing leaves the PC
  except what the agent itself puts in its conversation.
- Twelve tools, all read-only (`readOnlyHint: true`): `get_status`, `get_bottleneck`,
  `get_vram_forecast`, `get_processes`, `get_blackbox`, `get_history`,
  `compare_last_run`, `diagnose_crash`, `preflight`, `get_pcie_link`, `get_process_vram`
  and `get_job_reports`. They read the NVIDIA driver (NVML), the Windows Event Log and
  Tachyon's own files in `%LOCALAPPDATA%\Tachyon`. They never change clocks, fans, power
  limits, processes or files.
- Process and job names can say what you are working on; the skill tells the agent not to
  copy them elsewhere without asking.
- Tachyon releases come with `SHA256SUMS.txt` and a build provenance attestation
  (`gh attestation verify tachyon.exe --repo formray/tachyon`). Security model:
  [SECURITY.md](https://github.com/formray/tachyon/blob/main/SECURITY.md).

## Changes

- **1.2.0:** for Tachyon 1.2.0. The skill covers `get_job_reports` (render jobs a script
  marks with `tachyon mark start|end --job NAME`, compared with the previous run of the
  same job), `get_process_vram` (who held VRAM and when), `get_pcie_link` (idle power
  saving is normal; only a degraded link matters) and `diagnose_crash.last_seconds`
  (100 ms readings before a stop). The MCP server still never writes markers.
- **1.1.0:** for Tachyon 1.1.0. The skill covers `diagnose_crash` (probable cause of a crash
  or power-off, with evidence and advice) and `preflight` (will a render fit in VRAM?), and
  `get_blackbox`'s `since`/`until`/`session`.
- **1.0.1:** first release, for Tachyon 1.0.1.

## Licence

MIT OR Apache-2.0 ([LICENSE-MIT](LICENSE-MIT), [LICENSE-APACHE](LICENSE-APACHE)),
© 2026 Formray, like [Tachyon](https://github.com/formray/tachyon).

---

<sub>Made by [Formray](https://formray.io). Technology, thoughtfully.</sub>
