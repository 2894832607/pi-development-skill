# Pi official links and source map

Read only the sections needed by the current feature. Check these sources before writing new Pi infrastructure.

## Main repositories and learning entrypoints

- Pi official monorepo: <https://github.com/badlogic/pi-mono>
- Coding Agent package: <https://github.com/badlogic/pi-mono/tree/main/packages/coding-agent>
- Coding Agent source: <https://github.com/badlogic/pi-mono/tree/main/packages/coding-agent/src>
- Coding Agent documentation index: <https://github.com/badlogic/pi-mono/tree/main/packages/coding-agent/docs>
- Official SDK examples: <https://github.com/badlogic/pi-mono/tree/main/packages/coding-agent/examples/sdk>
- Official Extension examples: <https://github.com/badlogic/pi-mono/tree/main/packages/coding-agent/examples/extensions>
- Author-maintained Pi Skills: <https://github.com/badlogic/pi-skills>
- Agent Skills specification used by Pi: <https://agentskills.io/specification>

## Architecture and tutorials

- How Pi works and the agent loop: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/how-pi-works.md>
- Quickstart: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/quickstart.md>
- SDK guide: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/sdk.md>
- SDK example guide: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/examples/sdk/README.md>
- CLI integration: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/cli-integration.md>
- RPC guide: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/rpc.md>
- RPC commands: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/rpc-commands.md>
- RPC Extension UI: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/rpc-extension-ui.md>

## Extensions, tools, UI, and external capabilities

- Extension API, events, lifecycle, tools, and UI: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/extensions.md>
- Extension example catalog: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/examples/extensions/README.md>
- Terminal UI: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/tui.md>
- MCP: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/mcp.md>
- Codemode: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/codemode.md>
- Custom providers: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/custom-provider.md>
- Provider configuration: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/providers.md>
- Models: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/models.md>

## Sessions, messages, and context

- Sessions and context: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/sessions.md>
- Session file format: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/session-format.md>
- Message types: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/message-types.md>
- Compaction: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/compaction.md>
- Configuration: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/configuration.md>
- Settings: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/settings.md>
- Environment variables: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/environment-variables.md>

## Skills, prompts, commands, and packages

- Skills guide: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/skills.md>
- Pi Packages: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/packages.md>
- Prompt templates: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/prompt-templates.md>
- Slash commands: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/slash-commands.md>
- Command line reference: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/cli.md>

## Security and runtime behavior

- Security: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/security.md>
- JSON event mode: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/json.md>
- Containerization: <https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/containerization.md>

## Search for reusable code before implementing

Search the official repository by API, event, or capability, not only by product name. Useful locations:

```text
packages/coding-agent/src/
packages/coding-agent/examples/sdk/
packages/coding-agent/examples/extensions/
packages/coding-agent/docs/
packages/agent/
packages/ai/
```

For reusable third-party Pi resources, search npm, GitHub, and the package declarations already present in the project. Before using one, verify its source, peer dependencies, license, version compatibility, lifecycle, and cleanup behavior.

## Verify the target version

Prefer the target project's installed declarations and examples:

```text
node_modules/@earendil-works/pi-coding-agent/dist/**/*.d.ts
node_modules/@earendil-works/pi-coding-agent/examples/sdk/
node_modules/@earendil-works/pi-coding-agent/examples/extensions/
node_modules/@earendil-works/pi-agent-core/
node_modules/@earendil-works/pi-ai/
```

Useful read-only checks:

```bash
npm ls @earendil-works/pi-coding-agent @earendil-works/pi-agent-core @earendil-works/pi-ai
git -C <pi-mono-checkout> rev-parse HEAD
git -C <pi-mono-checkout> show -s --format=%cI HEAD
```

For a fresh official snapshot, shallow-clone the public repository into a temporary directory. Do not replace the project's pinned Pi version merely because `main` is newer.

## Verified snapshot

This snapshot is orientation only. Refresh it for future work:

- Verified on: 2026-11-06
- Pi monorepo commit: `2a32a605a9f18a5f0191f65358a2e764b2f69fc7`
- Commit date: 2026-11-05
- `@earendil-works/pi-coding-agent@1.5.0`
- `@earendil-works/pi-agent-core@1.5.0`
- `@earendil-works/pi-ai@1.5.0`
