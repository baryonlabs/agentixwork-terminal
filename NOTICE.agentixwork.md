# AgentixWork Terminal Notice and Attribution

AgentixWork Terminal is a fork of `manaflow-ai/cmux`.

Upstream:

- Repository: https://github.com/manaflow-ai/cmux
- License: GPL-3.0-or-later with a commercial license option
- Copyright: Copyright (c) 2024-present Manaflow, Inc.

This fork keeps the upstream `LICENSE`, copyright notices, and third-party license notices.

## Referenced Projects

### cmux

cmux is the base terminal application and provides:

- Native macOS terminal app architecture.
- Ghostty-based terminal rendering.
- Workspace, split, tab, notification, browser, file explorer, CLI, and socket primitives.

AgentixWork Terminal derives from cmux and must follow cmux's GPL-3.0-or-later license terms unless a commercial license is arranged with Manaflow.

### Zeude

Zeude is referenced as an architecture and product-direction input, not copied into this repository at this stage.

- Repository: https://github.com/zep-us/zeude
- License: Apache-2.0

Relevant ideas:

- CLI shim for Claude Code and Codex.
- Centralized skills, MCP, hooks, and agent profile sync.
- OpenTelemetry-based AI coding assistant usage analytics.
- Self-hosted dashboard direction.

No Zeude source code has been copied into AgentixWork Terminal in the current implementation. If Zeude code is imported later, preserve Apache-2.0 notices and document the exact files/modules imported.

### Neovim cmux setup

The Neovim file editing setup is referenced as the default recommended editor workflow.

- Repository: https://github.com/baryonlabs/cmux_setting_vscode_style_nvim
- License: MIT

AgentixWork Terminal should link to this setup and launch `nvim` in the resolved project directory. It should not silently overwrite user dotfiles.

## Current Import Policy

Allowed now:

- Referencing upstream repositories.
- Reusing cmux code through this fork under GPL-3.0-or-later.
- Adding original AgentixWork code to this fork.
- Adding documentation that points users to Zeude and the Neovim setup repo.

Not allowed without explicit tracking:

- Copying Zeude source files without preserving Apache-2.0 notices.
- Removing cmux copyright or license notices.
- Re-licensing this cmux-derived app as MIT.
- Silently installing or overwriting user dotfiles.

## Distribution Note

Because this is a cmux-derived work, public distribution should be treated as GPL-3.0-or-later unless a commercial license is obtained from Manaflow.
