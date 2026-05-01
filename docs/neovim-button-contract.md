# Neovim Button Contract

This document defines the first AgentixWork Terminal integration point for the Neovim setup published at:

https://github.com/baryonlabs/cmux_setting_vscode_style_nvim

## Goal

Give users a visible button that opens Neovim in the most relevant project directory without requiring them to remember terminal commands.

## User Story

1. A user works with Claude/Codex in AgentixWork Terminal.
2. The agent creates or edits files in a project directory.
3. The user clicks `Nvim`.
4. Neovim opens in that same directory.
5. The file explorer is available with `Ctrl+b`.

## Path Resolution

Use this order:

1. Focused terminal pane current directory.
2. Current file explorer root.
3. Workspace directory.
4. Last known directory for the workspace.
5. `$HOME`.

Never guess from a random recent directory if the focused workspace has a known path.

## Launch Modes

V1 should support one launch mode:

```sh
cd "$TARGET_DIR" && nvim
```

Later modes:

| Mode | Behavior |
| --- | --- |
| focused pane | Type/run `nvim` in current pane |
| new split | Create a split and run `nvim` |
| new workspace | Create a workspace and run `nvim` |

## Failure Handling

If `nvim` is missing:

- Show a short message.
- Offer install instructions.
- Link to the Neovim setup repo.

If the setup repo is not installed:

- Still launch regular `nvim`.
- Show setup as optional.

If a target path does not exist:

- Fall back to workspace directory.
- Then fall back to `$HOME`.

## Safety

Do not automatically overwrite:

- `~/.config/nvim/init.lua`
- `~/.config/nvim/cmux-base.lua`
- existing user dotfiles

Any future installer must back up existing files and show a dry-run summary first.
