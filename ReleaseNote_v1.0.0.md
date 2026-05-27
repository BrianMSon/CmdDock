# CmdDock v1.0.0 Release Notes

**Date:** 2026-05-27
**Status:** Initial Release

---

## Welcome to CmdDock!

The first official release of **CmdDock**, a lightweight Windows desktop utility that docks multiple CMD, PowerShell, Terminal, and Explorer windows into a single grid host.

Built on **Wails (Go + Svelte + WebView2)** for a tiny memory footprint and a native Win32 docking core.

---

## Key Features

### Grid Docking
- **Row × Column grid layout** — configurable cell grid; each cell can hold one docked window
- **Drag-to-dock** — drag any supported window over the host and drop into a cell
- **Live cell resize** — drag row/column borders; docked windows reposition in real time
- **Host follow** — moving the host window carries every docked child along (~50 ms tracking loop)
- **Undock / Redock** — release a cell back to a free-floating window, or re-bind a matching live window from saved session data
- **Alt+Tab MRU promotion** — undocked windows jump to the top of the Alt+Tab list

### Supported Window Types
- **cmd.exe** — classic Console Window (`ConsoleWindowClass`)
- **powershell.exe**
- **Windows Terminal** (`CASCADIA_HOSTING_WINDOW_CLASS`)
- **ConEmu** (`VirtualConsoleClass`)
- **Git Bash** (`mintty`)
- **File Explorer** (`CabinetWClass`)
- **Custom executables** — any `.exe` / `.bat` / `.cmd` / `.lnk` can be registered to a cell

### Multi-Instance Sessions
- **Up to 16 simultaneous instances**, each with an independent slot acquired via mutex
- **Per-slot session file** (`session-{N}.json`) persists:
  - Grid dimensions and column/row width ratios
  - Host window position and size
  - Per-cell window metadata (title, app type, PID, Explorer path, custom-exe path, label)
  - Theme color overrides
- **Redock All** — on relaunch, scans live top-level windows and re-binds matching ones to their saved cells without spawning new processes
- **Redock Cell** / **Reset Saved Cell** for fine-grained control
- Session format is **compatible with the original .NET CmdDock** build

### Explorer Integration
- Launch Explorer into a cell pointing at a saved path
- **`/separate` launch** — each docked Explorer runs in its own process, ensuring shell extension overlay handlers (e.g. TortoiseGit) load reliably regardless of Windows' 15-slot overlay limit
- Saved per-cell Explorer path, restored on Redock

### Per-Cell Customization
- Inline-editable **cell label** in the cell titlebar
- **Custom exe registration** — bind any executable path to a cell; survives transient cmd/PowerShell/Explorer docking and is restored on Redock
- Per-cell right-click menu: Launch CMD / PowerShell / Explorer / Custom, Pick Exe..., Undock, Redock, Reset

### Custom Titlebar & UI
- Self-drawn dark titlebar/toolbar (no native window frame)
- Window controls (Minimize / Maximize / Close) in the custom titlebar
- **Theme color overrides** for titlebar / toolbar / cell titlebar, persisted per slot
- Status bar with live launch / docking diagnostics

### Drag & Drop
- Global drag watcher detects external windows dragged over the host
- Drop targeting uses a centered hit-rect per cell so accidental drops on cell edges are ignored
- Drop is rejected if another window obscures the host at the drop point

---

## System Requirements

| Requirement | Specification |
|-------------|---------------|
| OS | Windows 10 or later |
| Architecture | x64 |
| Runtime | Microsoft Edge WebView2 Runtime (bundled on Windows 11; auto-installed on Windows 10 via Windows Update) |

---

## Installation

1. Download the release ZIP file
2. Extract to your preferred location
3. Run `CmdDock.exe`

No installation required — fully portable. Session files (`session-1.json` … `session-16.json`) are written next to the executable.

---

## Third-Party Libraries

This release uses the following open-source components:

- **Wails v2** — Go desktop application framework (MIT License)
- **Svelte** — frontend framework (MIT License)
- **golang.org/x/sys/windows** — Win32 system call bindings (BSD-3-Clause)

---

## Known Notes

- Some applications cannot be docked when CmdDock runs without elevation while the target window is elevated. Launch CmdDock as Administrator to dock elevated windows.
- Docked Explorer instances launched by CmdDock use `/separate` for clean overlay handler loading; this is intentional.

---

Thank you for using CmdDock!

_Copyright (c) 2026 HappySloth._
