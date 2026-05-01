# Upstream Issue Review for AgentixWork Terminal

Reviewed on 2026-05-01.

Upstreams:

- cmux: https://github.com/manaflow-ai/cmux
- zeude: https://github.com/zep-us/zeude

## Summary

AgentixWork Terminal should avoid building a separate editor/file-tree stack from scratch. cmux already has active issues and PRs around file explorer, native editor panels, preferred editor routing, current working directory tracking, and external CLI automation.

The safest first slice is:

1. Reuse cmux's existing file explorer.
2. Add a small `Nvim` launch action that resolves the focused terminal/workspace directory.
3. Keep Zeude integration as a later optional config/telemetry layer.

## cmux Issues Relevant to AgentixWork

| Issue | Status | Relevance |
| --- | --- | --- |
| [#3344 Integrated Code Workspace: File Explorer & Navigation Surface](https://github.com/manaflow-ai/cmux/issues/3344) | Open | Directly overlaps with AgentixWork's "file list + code navigation" goal. |
| [#888 File Explorer](https://github.com/manaflow-ai/cmux/issues/888) | Open | Original user demand for a right-sidebar file tree. Already partially addressed by merged PRs. |
| [#648 Add VSCode/Cursor like code editor as a type of screen](https://github.com/manaflow-ai/cmux/issues/648) | Open | Broad request for an editor surface. AgentixWork should not duplicate this with a separate editor. |
| [#1678 Configurable external editor for file paths](https://github.com/manaflow-ai/cmux/issues/1678) | Open | Strong match for a Neovim-based workflow. Mentions `nvim` as a possible editor command. |
| [#3156 Default working directory for new workspaces](https://github.com/manaflow-ai/cmux/issues/3156) | Open | Relevant to resolving the target path for the `Nvim` button. |
| [#1448 External terminal doesn't open in current directory](https://github.com/manaflow-ai/cmux/issues/1448) | Open | Confirms external app/path launch semantics are a known problem. |
| [#807 Support opening .command files, shell scripts, and directories](https://github.com/manaflow-ai/cmux/issues/807) | Open | Relevant for app launch, Finder, Linear, and OS-level integration. |
| [#3089 CLI rejects connections from outside terminal session](https://github.com/manaflow-ai/cmux/issues/3089) | Open | Important if AgentixWork needs external automation outside a cmux child process. |
| [#1864 Allow external CLI access via socket password authentication](https://github.com/manaflow-ai/cmux/issues/1864) | Open | Related to secure external control of cmux. |
| [#2772 Conditional keybinding passthrough for alt-screen apps](https://github.com/manaflow-ai/cmux/issues/2772) | Open | Important for Neovim inside cmux: shortcuts should pass through while `nvim` is active. |

## cmux PRs Relevant to AgentixWork

| PR | Status | Relevance |
| --- | --- | --- |
| [#1963 Add Finder-like file explorer sidebar with SSH support](https://github.com/manaflow-ai/cmux/pull/1963) | Merged | Existing foundation for file list button. Reuse this. |
| [#2843 feat: file explorer sidebar panel](https://github.com/manaflow-ai/cmux/pull/2843) | Open | Adds VSCode-style file explorer behavior and workspace CWD sync. Potential conflict area. |
| [#2316 File Explorer + Native Editor sidebar](https://github.com/manaflow-ai/cmux/pull/2316) | Open | Adds a native editor, git status, file search, external editor open. Large overlap. |
| [#2864 Add text editor panel for file explorer](https://github.com/manaflow-ai/cmux/pull/2864) | Open | Adds editor tabs/session restore. Avoid duplicating. |
| [#3042 Route Files panel opens through editor settings](https://github.com/manaflow-ai/cmux/pull/3042) | Open | Directly related to routing files through preferred editor settings. |
| [#3387 Add zoom shortcuts to file viewer](https://github.com/manaflow-ai/cmux/pull/3387) | Open | Shows the file viewer remains actively changing. Expect merge conflicts around file explorer UI. |
| [#3093 Allow external clients to connect to cmux socket](https://github.com/manaflow-ai/cmux/pull/3093) | Open | May resolve external automation blocker for AgentixWork. Security-sensitive. |
| [#3151 Add cmux agent launcher](https://github.com/manaflow-ai/cmux/pull/3151) | Open | Overlaps with AgentixWork's agent launcher/product direction. Review before designing another launcher. |

## zeude Issues Relevant to AgentixWork

| Issue | Status | Relevance |
| --- | --- | --- |
| [#9 Plugin & Complex Skill Synchronization](https://github.com/zep-us/zeude/issues/9) | Open | Best match for future AgentixWork config sync: skills, MCP servers, plugins. |
| [#25 Align efficiency metrics with Claude/Codex best practices](https://github.com/zep-us/zeude/issues/25) | Open | Relevant to telemetry and multi-tool usage analysis. Do not pull this into V1. |
| [#7 Personal Skill Usage Analytics](https://github.com/zep-us/zeude/issues/7) | Open | Later dashboard/personal insights idea. |
| [#8 Team-level Skill Usage Analytics](https://github.com/zep-us/zeude/issues/8) | Open | Later team dashboard idea. |
| [#10 Slack Usage Reports](https://github.com/zep-us/zeude/issues/10) | Open | Notification/reporting layer, not V1 terminal integration. |

## Implications for AgentixWork

### Do not duplicate cmux editor work

cmux has multiple active file explorer/editor PRs. AgentixWork should use the upstream file explorer surface and add only the smallest Neovim-specific entry point.

Recommended first implementation:

- `AgentixWorkTargetPathResolver`
- `Nvim` button action
- Path resolution tests
- UI button in/near existing file explorer/sidebar controls

Avoid in V1:

- New file tree implementation
- New inline editor implementation
- New Monaco/TextKit editor surface
- Broad command palette changes

### Watch socket and external launch work

External automation is currently an open cmux concern. If AgentixWork wants URL handlers, Dock buttons, Raycast/Alfred, or external controller processes, track:

- cmux issue #3089
- cmux issue #1864
- cmux PR #3093

Until this stabilizes, prefer actions initiated inside the running app process rather than relying on external socket clients.

### Neovim-specific concern

For a first-class Neovim workflow, track:

- cmux issue #2772 for alt-screen shortcut passthrough.
- cmux issue #1678 for preferred external editor routing.

If `Nvim` runs inside cmux, shortcut passthrough matters more than external editor routing.
If `Nvim` runs as an external editor command, preferred editor routing matters more.

## Recommended Next Step

Implement only a local fork slice:

1. Add a pure path resolver with tests.
2. Add a command builder that returns `cd "$path" && nvim`.
3. Add a prototype `Nvim` button that uses existing workspace/panel directory state.
4. Do not touch file explorer internals beyond wiring the action.

After that, rebase against upstream cmux and check conflicts with PRs #2843, #3042, #3387, and #3151.
