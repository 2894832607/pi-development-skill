---
name: pi-development
description: Design, build, integrate, debug, or review agents based on the Pi coding agent. Use for Pi SDK embedding, extensions, custom tools, MCP, sessions, RPC or web UI, skills, packages, subagents, and Pi runtime upgrades. Do not use for agent systems that do not use Pi.
metadata:
  version: "0.1.0"
  last-verified: "2026-11-06"
---

# Pi Agent development

Build on Pi's current public APIs and reference implementation. Reuse Pi's session, agent loop, tools, resources, and lifecycle before writing new infrastructure.

## Start every feature with a reuse check

Before designing or changing code:

1. Identify the exact Pi packages and versions used by the target project from `package.json`, its lockfile, and the installed dependency tree.
2. Open [references/official-sources.md](references/official-sources.md). Read the current official docs, source, and examples related to the requested feature.
3. Search the official extension examples, SDK examples, official Skills repository, and Pi Packages for an existing implementation or primitive.
4. Inspect the matching installed TypeScript declarations. They are the contract for the project even when official `main` has newer APIs.
5. Write down what can be reused and what gap still requires custom code. Only implement that gap.

If the installed version and current official `main` differ, implement against the installed version unless the user also requested an upgrade. Do not silently copy an API that exists only on `main`.

## Choose the smallest Pi primitive

| Need | Prefer |
|---|---|
| On-demand instructions, scripts, or references | Skill |
| Reusable input text | Prompt template |
| Tools, commands, lifecycle hooks, providers, gates, or terminal UI | Extension |
| An application embedding Pi in the same TypeScript process | SDK |
| A separate process or language-independent client | RPC, JSON mode, or CLI integration |
| External service tools | Pi's MCP support or a thin extension adapter |
| Reusable distribution of extensions, skills, prompts, and themes | Pi Package |

Do not create a custom agent loop, transcript format, event bus, skill loader, tool dispatcher, compaction layer, or session store when Pi already supplies the required behavior.

## Reuse in this order

1. Existing Pi resources already present in the target project.
2. Public APIs and built-ins in the project's installed Pi version.
3. A typechecked official example with the same lifecycle and runtime mode.
4. An official or maintained Pi Skill, Extension, or Package whose behavior and version fit.
5. Minimal custom code for the remaining gap.

Treat community packages as implementation dependencies, not API authority. Read their source, license, peer dependencies, startup behavior, cleanup behavior, and security boundary before adopting them.

## Design before coding

Write a short implementation contract that states:

- target Pi version and runtime mode;
- the Pi primitive being used and why;
- official example, extension, Skill, Package, or API being reused;
- input, output, session, and tool contracts;
- long-lived resources and who closes them;
- observable acceptance checks.

Keep configuration in the host project's existing source of truth. Do not spread the same setting across code, environment files, and package metadata unless an external package requires an environment bridge; derive that bridge from host configuration.

## Implementation rules

- Use exported Pi types and the installed `.d.ts` files for exact signatures. Do not infer APIs from names or old tutorials.
- Use `DefaultResourceLoader` for normal resource discovery with focused overrides. Replace the loader only when the host truly owns resource storage and discovery.
- Use `SessionManager` as the source of persisted conversation state. Do not mutate model message state as a substitute for session history.
- Subscribe before prompting when streamed events matter. Use completed message events for final content and `agent_settled` when the host needs a terminal idle signal.
- Dispose every SDK session. Start long-lived extension resources on the documented lifecycle event and close them on `session_shutdown`.
- Register tools with typed schemas and preserve Pi's expected result shapes. Reuse built-in renderers and tool definitions when overriding built-ins.
- Keep tools focused on external actions or deterministic computation. Put orchestration policy in the system prompt, a Skill, or a small extension hook only when the runtime needs it.
- Distinguish model-returned thinking blocks from hidden chain of thought. A UI may render returned reasoning events; it cannot promise access to undisclosed internal reasoning.
- Treat tool execution success as execution evidence only. Validate parameters, entity selection, returned fields, and how the evidence supports the final answer.

Read [references/engineering-playbook.md](references/engineering-playbook.md) for task-to-example routing and acceptance checks.

## Test at the real boundary

Use the narrowest tests that prove the integration:

1. Typecheck against the pinned Pi version.
2. Load resources and assert there are no extension or Skill diagnostics.
3. Exercise session startup, one representative prompt or tool call, streamed events, and shutdown.
4. Test every runtime mode the feature claims to support. Respect `ctx.mode` and `ctx.hasUI`.
5. Confirm abort, timeout, cleanup, and session persistence when the feature owns those behaviors.

For a UI or remote-control integration, test the actual HTTP or WebSocket endpoint and one browser command. A successful page load alone does not prove that prompts, tool events, or session state are connected.

## Delivery

Report:

- what Pi already provided and what custom code remains;
- exact files changed;
- version and official sources checked;
- tests and real smoke results;
- unsupported modes or known package limitations.

If an API cannot be verified in the target version, stop and mark it unverified. Do not add compatibility wrappers or fallback branches based on guesses.

## Keep this Skill current

When updating this Skill, refresh the official repository, inspect the current docs tree and package versions, update the verified snapshot in [references/official-sources.md](references/official-sources.md), and rerun the Skill validator. Keep canonical links stable in the Skill; put time-specific commits and versions in the reference.
