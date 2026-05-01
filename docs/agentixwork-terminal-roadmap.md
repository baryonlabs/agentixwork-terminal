# AgentixWork Terminal Roadmap

AgentixWork Terminal is planned as a cmux fork that adds a first-class file editing workflow for AI coding agents.

Base:

- Upstream terminal app: `manaflow-ai/cmux`
- Monitoring/config inspiration: `zep-us/zeude`
- Neovim setup: `baryonlabs/cmux_setting_vscode_style_nvim`

## License note

cmux is GPL-3.0-or-later with a commercial option. A distributed fork that derives from cmux must follow the GPL terms unless a commercial license is arranged with Manaflow.

zeude is Apache-2.0. Apache-2.0 code and ideas can be combined into a GPL-licensed derivative, but the final combined app should be treated as GPL-compatible, not MIT.

## Product direction

AgentixWork Terminal should keep cmux's terminal-first model and add opinionated file editing controls for users who run Claude Code, Codex, OpenCode, Gemini, and other coding agents.

The first version should not replace cmux's core primitives. It should add a thin workflow layer:

- A visible `Nvim` button in the file/sidebar area.
- A file list button that opens or focuses cmux's existing file explorer.
- A "last path" resolver that launches the editor in the directory most recently used by the focused terminal pane.
- Optional Zeude-style telemetry/config hooks later, after the local editor workflow is stable.

## V1 Scope

### 1. Nvim Button

Add a button that launches Neovim inside the focused terminal or a new split.

Expected behavior:

1. Resolve the target directory.
2. Prefer the focused terminal pane's current directory.
3. Fall back to the workspace directory.
4. Fall back to `$HOME`.
5. Run `nvim` in that directory.

Command shape:

```sh
cd "$AGENTIXWORK_TARGET_DIR" && nvim
```

If the user has installed the Neovim setup from `baryonlabs/cmux_setting_vscode_style_nvim`, Neovim should start with:

- Neo-tree available with `Ctrl+b`.
- Markdown preview available from the right-click menu.
- Korean command correction.
- Startup status showing current folder and Git status.

### 2. File List Button

Add a button near the terminal/sidebar controls that opens the file explorer for the resolved target directory.

Use existing cmux pieces first:

- `FileExplorerView.swift`
- `FileExplorerStore.swift`
- `Workspace.panelDirectories`
- `Workspace.sidebarResolvedDirectory`
- Existing `file.open` socket/CLI behavior

The button should not introduce a separate file tree implementation.

### 3. Last Path Resolver

cmux already receives terminal current-directory updates. AgentixWork should wrap this into a small resolver:

```swift
struct AgentixWorkTargetPathResolver {
    func targetDirectory(
        focusedPanelDirectory: String?,
        workspaceDirectory: String?,
        fallbackHome: String
    ) -> String
}
```

Resolution order:

1. Focused terminal pane current directory.
2. Selected file explorer root.
3. Workspace current directory.
4. Last known directory persisted for this workspace.
5. `$HOME`.

### 4. Button Contract

The first UI can be intentionally small:

| Button | Action |
| --- | --- |
| `Files` | Focus/open file explorer at target path |
| `Nvim` | Launch `nvim` at target path |
| `README` | Open README/Markdown preview when available |

## Zeude Integration Direction

Do not merge zeude wholesale in V1.

Use zeude as a design reference for:

- Agent CLI shim concepts.
- Team configuration sync.
- Skills/MCP/hook distribution.
- Usage telemetry via OpenTelemetry.

Possible V2 modules:

- `AgentixWork Shim`: wraps `claude`, `codex`, and other CLIs.
- `AgentixWork Dashboard`: self-hosted team dashboard based on zeude concepts.
- `AgentixWork Config Sync`: pushes skills, MCP servers, hooks, and Neovim setup instructions.
- `AgentixWork Telemetry`: tracks agent usage and editor launch context.

## Implementation Slices

### Slice A: Branding and docs

- Rename user-facing app strings from cmux to AgentixWork Terminal where appropriate.
- Preserve upstream license notices.
- Add this roadmap and integration docs.

### Slice B: Nvim launch action

- Add target path resolver.
- Add launch command builder.
- Add UI button.
- Add tests for path resolution.

### Slice C: File list button

- Reuse existing file explorer.
- Wire button to current workspace/pane directory.
- Verify local and SSH behavior separately.

### Slice D: Neovim setup install helper

- Add a command or button that points users to:
  `https://github.com/baryonlabs/cmux_setting_vscode_style_nvim`
- Do not silently overwrite `~/.config/nvim/init.lua`.
- Offer backup and dry-run behavior before install.

### Slice E: Zeude-style integration

- Add telemetry/config sync only after local workflow is stable.
- Keep it opt-in.
- Document all data collection.

## Open Questions

- Should `Nvim` open in the focused pane, a new split, or a new workspace by default?
- Should the app install the Neovim setup, or only link to the setup repo?
- Should AgentixWork use GPL-only distribution or pursue a cmux commercial license?
- Which agent CLIs are first-class: Claude, Codex, OpenCode, Gemini?
- Should Zeude remain a sidecar service or be embedded as an optional dashboard?

## Upstream Review

Before implementation, review the related upstream cmux and zeude issues in:

```text
docs/upstream-issue-review.md
```

The key conclusion is to reuse cmux's existing file explorer and add only a narrow `Nvim` launch action first.
