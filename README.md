# omarchy-dotfiles-sync

Auto-sync chezmoi dotfiles to git remote.

## Requirements

- Omarchy (or Arch with inotify-tools)
- chezmoi
- git remote configured in chezmoi repo

## Install

```bash
git clone https://github.com/WillyV3/omarchy-dotfiles-sync
cd omarchy-dotfiles-sync
./setup
```

## How it works

1. `inotifywait` monitors `~/.local/share/chezmoi`
2. On file change, starts 20-minute timer
3. More changes reset timer
4. After 20 minutes of no changes, commits and pushes

## Configuration

Edit `~/.config/systemd/user/dotfiles-sync.service`:

```ini
Environment="DEBOUNCE_SECONDS=1200"  # 20 minutes
```

Then reload:
```bash
systemctl --user daemon-reload
systemctl --user restart dotfiles-sync
```

## Commands

```bash
systemctl --user status dotfiles-sync    # status
systemctl --user stop dotfiles-sync      # stop
systemctl --user start dotfiles-sync     # start
tail ~/.local/state/dotfiles-sync.log    # logs
```

## Uninstall

```bash
systemctl --user stop dotfiles-sync
systemctl --user disable dotfiles-sync
rm ~/.local/bin/dotfiles-sync
rm ~/.config/systemd/user/dotfiles-sync.service
```

## Files

```
dotfiles-sync          # daemon script
dotfiles-sync.service  # systemd unit
setup                  # installer
```
