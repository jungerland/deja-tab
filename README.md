# deja-tab

You've seen these tabs before.

`deja-tab` saves the Claude Code sessions you have open in iTerm2, and after you restart iTerm2 (or your Mac), it types `claude --resume` back into each tab, in the same window, the same tab, and the same pane. Then it renames each tab to the session's title.

macOS and iTerm2 only. One Python file, standard library only, nothing to install.

## Install

```
git clone https://github.com/jungerland/deja-tab.git ~/deja-tab
mkdir -p ~/.local/bin && ln -s ~/deja-tab/deja-tab ~/.local/bin/deja-tab
```

Make sure `~/.local/bin` is on your PATH (or link it somewhere that is). The first run asks you to let your terminal control iTerm2. Say yes.

## One-time setup in iTerm2

deja-tab brings back your sessions. iTerm2 brings back your windows. You have to set that half up once:

1. Settings > General > Startup > Window restoration policy: **Open Default Window Arrangement**.
2. Arrange your windows and tabs the way you like them, then Window > Save Window Arrangement, name it `claude-sessions`.
3. Settings > Arrangements, select `claude-sessions`, and click **Set Default**.

Name your windows if you have more than one (Window > Edit Window Title). deja-tab finds each window by its exact name, so "Work" and "WORK" are different windows.

## Every restart

Before:

1. Window > Save Window Arrangement, name it `claude-sessions` (overwrite the old one).
2. `deja-tab snapshot`

It warns you if the arrangement doesn't match the windows you have open. If it does, save the arrangement again and rerun the snapshot.

After:

3. Do nothing while iTerm2 opens. It restores the `claude-sessions` arrangement by itself. **Don't** also restore it by hand, or you get every window twice.
4. `deja-tab restore`

Try `deja-tab restore --dry-run` first to see what it will type.

## Commands

| Command | What it does |
|---|---|
| `deja-tab snapshot` | Save the open sessions to `~/.claude/session-snapshots/` |
| `deja-tab list` | Show what the newest snapshot holds |
| `deja-tab restore` | Resume each session in its old tab |
| `deja-tab restore --dry-run` | Print the commands, type nothing |

`restore` skips any tab that already runs Claude, so running it twice is safe. A session that never got far enough to save a transcript reopens as a fresh `claude` in the same folder.

To use an arrangement name other than `claude-sessions`, set `DEJA_TAB_ARRANGEMENT`.

## Why two halves

iTerm2 has no scripting command to save or restore a window arrangement, and window titles and tab groups can't be set from a script at all. Only a saved arrangement carries them. So iTerm2 rebuilds the windows, and deja-tab fills them.

## Caveats

- deja-tab reads `~/.claude/sessions/`, a folder Claude Code uses internally and doesn't document. A Claude Code update could change it and break deja-tab without warning.
- It only knows about sessions running in iTerm2 tabs. Sessions in other terminals are listed but have no tab to go back to.
- If you add or close tabs after saving the arrangement, tabs won't line up. Save the arrangement right before the snapshot.

## License

MIT
