<!-- BEGIN HYPERAISTUDIO MANAGED BLOCK -->
# HyperAIStudio Agent Instructions

## Project

- Root: `F:/Documents/Unreal Projects/MyProject3/`
- Native MCP: `unreal-mcp` at `http://127.0.0.1:8000/mcp`

- Use native MCP for Unreal editor actions and the generated Start/Wait helpers for lifecycle and readiness checks; do not hand-roll MCP protocol calls.
- Follow the user's requested intent first. If asked to start, open, or reconnect the project, run `.hyperai/scripts/Start-HyperAIStudioEditor.ps1`, wait for it to report MCP ready, and then continue.
- Missing native MCP tools do not prove that the agent was opened from the wrong directory. Start and verify Unreal first.
- After readiness, use native `unreal-mcp` tools when the current agent client exposes them. If it still does not, continue work that does not require Unreal tools; only ask for an MCP reload or project-root reopen when the requested task actually requires those tools.
- Never ask for a new agent session merely to start Unreal Editor, and never claim that editor startup failed only because native tools are absent from the current session.
- Use normal filesystem tools for source files.
- Do not overlap Unreal MCP calls.

## Fast Paths

- Current level: `call_tool` -> `editor_toolset.toolsets.scene.SceneTools` / `get_current_level` / `{}`.
- List actors: `call_tool` -> `editor_toolset.toolsets.scene.SceneTools` / `find_actors` / `{"name":"","tag":"","collision_channels":[]}`.

## Reconnect

- If Unreal is closed or disconnected, run `.hyperai/scripts/Start-HyperAIStudioEditor.ps1`; this also waits for MCP readiness.
- Use `.hyperai/scripts/Wait-HyperAIStudioMCP.ps1 -TimeoutSeconds 90` for a readiness-only retry, then continue the requested task.

## Context Packs

- Only read `.hyperai/context/` files when the prompt explicitly references them.

## Shared Project Workspace

- Treat `.hyperai/` as the shared, Git-trackable workspace for useful agent context, prompts, plans, research, decisions, logs, and task-local scratch files.
- Do not create generic agent-work folders such as `Planning`, `Temp`, `Research`, `Notes`, or `Scripts` in the project root.
- For substantial work, create `.hyperai/work/YYYY-MM-DD-<clear-context-name>/` using a clear lowercase kebab-case context name. Add only the useful parts: `README.md`, `plan.md`, `context/`, `prompts/`, `research/`, `decisions/`, `log/`, or `temp/`.
- Name architecture and product decisions `decisions/YYYY-MM-DD-<system>-<decision>.md`. After meaningful implementation or design work, add a short dated entry under `log/` with what changed, decisions made, relevant files, and follow-up work.
- Do not create history for simple questions or read-only checks. Consult `.hyperai/INDEX.md` only when prior project context is relevant; do not scan all history every turn.
- Keep real source, content, config, and other project-owned files in their normal project locations. Never move an existing project folder merely because its name resembles an agent-work folder.
- Do not store secrets, credentials, large generated binaries, or unfiltered terminal dumps in `.hyperai/`.

## Extra MCP Servers

- No extra MCP servers are enabled yet.

Use extra MCP servers only when relevant to the task.

<!-- END HYPERAISTUDIO MANAGED BLOCK -->
