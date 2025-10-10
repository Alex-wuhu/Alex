---
title: Tmux Cheat Sheet
date: 2025-10-10
type: note
tags:
  - CLI
---

A quick reference guide for the most common `tmux` commands and shortcuts.

[[toc]]

> [!IMPORTANT]
> The default prefix for all commands is `Ctrl+b`. Press the prefix combination, release it, and then press the command key.

## Session Management

Tmux sessions allow you to manage multiple terminal environments.

| Command                       | Description                      |
| ----------------------------- | -------------------------------- |
| `tmux new -s <name>`          | Create a new named session.      |
| `tmux ls`                     | List all active sessions.        |
| `tmux attach -t <name>`       | Attach to a named session.       |
| `tmux kill-session -t <name>` | Kill a named session.            |
| `Ctrl+b d`                    | Detach from the current session. |
| `Ctrl+b $`                    | Rename the current session.      |

## Window Management

Windows are like tabs in a browser.

| Shortcut          | Description                                          |
| ----------------- | ---------------------------------------------------- |
| `Ctrl+b c`        | Create a new window.                                 |
| `Ctrl+b ,`        | Rename the current window.                           |
| `Ctrl+b p`        | Switch to the previous window.                       |
| `Ctrl+b n`        | Switch to the next window.                           |
| `Ctrl+b <number>` | Switch to a window by its number (e.g., `Ctrl+b 1`). |
| `Ctrl+b w`        | List all windows.                                    |
| `Ctrl+b &`        | Kill the current window.                             |

## Pane Management

Panes allow you to split a window into multiple terminals.

| Shortcut             | Description                              |
| -------------------- | ---------------------------------------- |
| `Ctrl+b %`           | Split the current pane horizontally.     |
| `Ctrl+b "`           | Split the current pane vertically.       |
| `Ctrl+b o`           | Switch to the next pane.                 |
| `Ctrl+b <arrow-key>` | Navigate between panes using arrow keys. |
| `Ctrl+b q`           | Briefly show pane numbers.               |
| `Ctrl+b x`           | Kill the current pane.                   |
| `Ctrl+b z`           | Toggle zoom for the current pane.        |
| `Ctrl+b {`           | Swap with the previous pane.             |
| `Ctrl+b }`           | Swap with the next pane.                 |

## Copy Mode

Tmux has a "copy mode" that allows you to scroll back and copy text from the terminal history.

- **Enter Copy Mode**: `Ctrl+b [`
- **Navigate**: Use arrow keys, `PageUp`, `PageDown`.
- **Start Selection**: `Space`
- **Copy Selection**: `Enter`
- **Paste**: `Ctrl+b ]`
- **Exit Copy Mode**: `q`

## Other Useful Commands

| Shortcut   | Description                          |
| ---------- | ------------------------------------ |
| `Ctrl+b ?` | Show all key bindings.               |
| `Ctrl+b :` | Enter the tmux command prompt.       |
| `Ctrl+b r` | Force redraw of the attached client. |
