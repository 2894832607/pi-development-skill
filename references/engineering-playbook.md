# Pi Agent engineering playbook

Use this reference after selecting the Pi primitive in `SKILL.md`.

## Route work to official examples

| Task | Read first | Useful official examples |
|---|---|---|
| Embed a Pi session in an app | `docs/sdk.md` | `sdk/01-minimal.ts`, `02-custom-model.ts`, `12-full-control.ts` |
| Persist, resume, fork, or replace sessions | `docs/sessions.md`, `docs/session-format.md` | `sdk/11-sessions.ts`, `13-session-runtime.ts` |
| Add Skills or custom resource discovery | `docs/skills.md`, SDK resources section | `sdk/04-skills.ts`, `extensions/dynamic-resources/` |
| Register tools | `docs/extensions.md` custom tools section | `extensions/hello.ts`, `tools.ts`, `structured-output.ts` |
| Intercept or gate tool calls | Extension events and error handling | `permission-gate.ts`, `protected-paths.ts`, `confirm-destructive.ts` |
| Add MCP tools | `docs/mcp.md`, SDK codemode and MCP section | `sdk/14-codemode-mcp.ts` |
| Add commands or injected messages | Extension API methods | `commands.ts`, `send-user-message.ts`, `reload-runtime.ts` |
| Add terminal UI | Extension custom UI and `docs/tui.md` | `custom-header.ts`, `custom-footer.ts`, `widget-placement.ts` |
| Build a web or separate-process client | `docs/rpc.md`, `docs/rpc-extension-ui.md` | `rpc-client.ts`, `rpc-extension-ui.ts` |
| Add subagents | Extension example catalog | `extensions/subagent/` |
| Modify provider payload or add a provider | Provider and Extension docs | `provider-payload.ts`, `custom-provider-*` |
| Package and share customizations | `docs/packages.md` | Pi Package declarations in official examples |

Paths are relative to `packages/coding-agent/` in `badlogic/pi-mono`.

## SDK boundary

A host application normally owns:

- model and credentials;
- configuration and dependency versions;
- `SessionManager` selection;
- `DefaultResourceLoader` options;
- active tool selection;
- event transport to its UI;
- session disposal.

Pi should continue to own:

- the agent loop;
- model and tool turns;
- queued steering and follow-up messages;
- session branch and compaction semantics;
- extension event dispatch;
- Skill and prompt-template expansion.

Subscribe before `prompt()` when the host streams output. Use the event stream for progress and the completed session message for final content. Keep one `AgentSession` per live conversation unless the product explicitly needs a different boundary.

## Extension lifecycle

An Extension factory registers handlers, tools, commands, providers, and renderers. Keep registration deterministic.

- Create or connect long-lived resources during the documented startup phase.
- Store state inside the factory closure or session entries according to whether it must survive reload or branching.
- Stop timers, watchers, sockets, servers, and external clients on `session_shutdown`.
- Use `ctx.mode` and `ctx.hasUI` before relying on UI behavior.
- Use event names and payload types from the target version.

If a feature needs one listening port, load that Extension into one long-lived session. Do not attach it to every short-lived request session.

## Tool design

Use TypeBox schemas and concrete descriptions. A useful tool contract states:

- what evidence or action it supplies;
- required identifiers and how to obtain them;
- fields returned;
- whether it reads, writes, or reaches an open world;
- when another tool is a better fit.

Keep adapters thin. Normalize external data at the boundary, then let internal code use trusted typed values. Do not hide tool errors or turn missing evidence into a positive result.

When a tool calls other tools, use the nested-call API supported by the target version. Preserve parent and child tool-call observability and return the evidence the model needs.

## Skills and prompts

Use a Skill for procedural guidance that should load on demand. Keep its description discriminating. Put conditional detail in `references/`, deterministic repeated work in `scripts/`, and output templates in `assets/`.

Use a prompt template for reusable input text. Use the system prompt for stable agent identity and global behavior. Avoid duplicating the same policy in all three places.

## Acceptance checks

Choose checks that match the feature's claims:

- dependency versions and lockfile are stable;
- TypeScript compiles against the pinned version;
- resource loading reports no diagnostics;
- expected Skills, commands, tools, and handlers are present;
- a real session starts and receives one prompt;
- text, model-returned thinking, and tool events reach the intended UI;
- steering, follow-up, abort, timeout, and idle semantics work when exposed;
- session history persists or stays in memory as designed;
- sockets, timers, and external connections close after disposal;
- unsupported runtime modes fail clearly or are documented.

Keep smoke inputs small. A test that invokes a paid or rate-limited model should prove an integration property that unit tests cannot.

## Review questions

1. Which custom module can be deleted because Pi already owns that behavior?
2. Does the code match the installed version or only the latest docs?
3. Is session state stored through `SessionManager`?
4. Can every long-lived resource be traced to a shutdown path?
5. Do test results prove the claimed runtime behavior rather than only successful initialization?
