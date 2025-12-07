# omarchy-dotfiles-sync

Auto-sync chezmoi dotfiles to git remote. Polls for changes, auto-adds new files, sends desktop notification on sync.

## Features

- **Polls every 60s** (configurable) - checks for changes and syncs
- **Auto-adds** new files in tracked directories
- **Desktop notification** via libnotify on successful push
- Uses `chezmoi re-add` for efficient updates

## Requirements

- Arch Linux (or similar)
- chezmoi with git remote configured
- libnotify (optional, for notifications)

## Install

```bash
git clone https://github.com/williavs/omarchy-dotfiles-sync
cd omarchy-dotfiles-sync
./setup
```

## How it works

1. Runs `chezmoi re-add` to sync modified files
2. Auto-adds new files in tracked directories via `chezmoi unmanaged`
3. Commits and pushes to remote if changes exist
4. Shows desktop notification on success
5. Repeats every 60 seconds

## Configuration

Edit `~/.config/systemd/user/dotfiles-sync.service`:

```ini
Environment="DEBOUNCE=60"  # seconds between sync checks
```

For notifications on Wayland, ensure these are set (adjust as needed):
```ini
Environment="DISPLAY=:0" "WAYLAND_DISPLAY=wayland-1" "XDG_RUNTIME_DIR=/run/user/1000"
```

Then reload:
```bash
systemctl --user daemon-reload
systemctl --user restart dotfiles-sync
```

## Commands

```bash
systemctl --user status dotfiles-sync    # status
systemctl --user restart dotfiles-sync   # restart
systemctl --user stop dotfiles-sync      # stop
tail -f ~/.local/state/dotfiles-sync.log # watch logs
```

## Ignoring files

Add patterns to `~/.local/share/chezmoi/.chezmoiignore`:

```
# Example: ignore Claude Code state files
.claude/history.jsonl
.claude/todos/
.claude/debug/
```

## Uninstall

```bash
systemctl --user stop dotfiles-sync
systemctl --user disable dotfiles-sync
rm ~/.local/bin/dotfiles-sync
rm ~/.config/systemd/user/dotfiles-sync.service
```
