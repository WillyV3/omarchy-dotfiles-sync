# omarchy-dotfiles-sync

Auto-sync chezmoi dotfiles to git remote. Watches source files, auto-adds new files, sends desktop notification on sync.

## Features

- Watches **source files** (not just chezmoi dir) - edit `~/.config/hypr/bindings.conf`, it syncs
- **Auto-adds** new files in tracked directories
- **60s debounce** - waits for quiet period before syncing
- **Desktop notification** via libnotify on successful push
- Uses `chezmoi re-add` for efficient updates

## Requirements

- Omarchy (or Arch with inotify-tools)
- chezmoi with git remote configured
- libnotify (optional, for notifications)

## Install

```bash
git clone https://github.com/williavs/omarchy-dotfiles-sync
cd omarchy-dotfiles-sync
./setup
```

## How it works

1. Watches all tracked source directories + chezmoi dir
2. On file change, waits 60s for more changes (debounce)
3. Runs `chezmoi re-add` to sync modified files
4. Auto-adds new files in tracked directories
5. Commits and pushes to remote
6. Shows desktop notification on success

## Configuration

Edit `~/.config/systemd/user/dotfiles-sync.service`:

```ini
Environment="DEBOUNCE=60"  # seconds to wait after last change
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
# Claude Code state (don't track)
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
