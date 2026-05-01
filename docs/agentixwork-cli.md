# AgentixWork CLI

AgentixWork CLI is the command-line interface for AgentixWork Terminal.

The long-term goal is an OS-level terminal control surface for AI coding workflows:

- Open project workspaces.
- Focus files.
- Launch Neovim in the right directory.
- Start agent sessions.
- Connect future Zeude-style config and telemetry.

## Prototype

A shell prototype lives at:

```text
Prototypes/agentixwork-cli/agentixwork
```

It is intentionally small and should later be replaced by a Swift implementation inside the app bundle.

## Commands

### `agentixwork new <name|path> [--nvim] [--no-open] [--git|--no-git]`

Create a new project folder and start working in it.

Default behavior:

- A simple project name is created under `$AGENTIXWORK_PROJECTS_DIR`, or `~/dev` when the variable is not set.
- A path such as `./apps/demo` or `/work/demo` is used directly.
- Existing directories are reused.
- `git init` runs when `git` is available and the folder is not already a Git repository.
- The project opens with `cmux "$TARGET_DIR"` when `cmux` is available.

Examples:

```sh
agentixwork new my-app
agentixwork new ~/dev/my-app --nvim
agentixwork new ./scratch/demo --no-open --no-git
```

This command is the first prototype for the future "new project" button in AgentixWork Terminal.

### `agentixwork nvim [path]`

Open Neovim in the target directory.

Resolution:

1. Explicit `path` argument.
2. `$AGENTIXWORK_TARGET_DIR`.
3. `$CMUX_WORKSPACE_CWD`.
4. `$PWD`.
5. `$HOME`.

Command run:

```sh
cd "$TARGET_DIR" && nvim
```

### `agentixwork files [path]`

Open or focus AgentixWork/cmux file explorer for a path.

Prototype behavior:

- If `cmux` CLI is available, run `cmux open "$TARGET_DIR"`.
- If unavailable, print the resolved target path.

Future behavior:

- Use the app socket API directly.
- Focus the existing file explorer surface.
- Avoid creating duplicate workspaces when the path is already open.

### `agentixwork open [path]`

Open a directory in AgentixWork/cmux.

Prototype behavior:

```sh
cmux "$TARGET_DIR"
```

### `agentixwork doctor`

Check local prerequisites:

- `cmux`
- `nvim`
- Neovim setup repo link

## OS-Level Integration Targets

Future integrations:

- Finder/Open With support for directories.
- URL scheme: `agentixwork://open?path=...`
- Raycast/Alfred command.
- Dock button / toolbar button inside the app.
- Shortcut/automation entry point.

## Relationship to cmux CLI

The existing cmux CLI remains the low-level primitive.

AgentixWork CLI should be a workflow CLI:

| Low-level cmux | AgentixWork wrapper |
| --- | --- |
| `mkdir -p <path>` + `cmux <path>` | `agentixwork new <name|path>` |
| `cmux <path>` | `agentixwork open <path>` |
| socket APIs | `agentixwork files <path>` |
| custom commands | `agentixwork nvim <path>` |
| agent launcher | future `agentixwork agent ...` |

## Safety Rules

- Do not overwrite editor configs.
- Do not install Neovim setup without explicit confirmation.
- Prefer opening in the currently focused workspace when possible.
- Preserve cmux socket security settings.
