# Artfct plugin

This repo is the Artfct plugin. It is one package directory that loads in two formats:

- **Agent Plugins 1.0.0** for Codex, Cursor, VS Code, GitHub Copilot, OpenClaw, Hermes and the other [compatible clients](https://agent-plugins.org/compatible-clients).
- **The Claude plugin format** for Claude Code, the Claude app, Cowork and Grok Build, through the marketplace in `.claude-plugin/marketplace.json`.

This repo owns the skills. Edit them here, not in the Artfct app.

## Layout

```text
.claude-plugin/marketplace.json     Claude marketplace (points at ./plugins/artfct)
plugins/artfct/                     the package root
├── plugin.json                     Agent Plugins manifest
├── mcp.json                        Agent Plugins MCP config (streamable-http)
├── .claude-plugin/plugin.json      Claude manifest
├── .mcp.json                       Claude MCP config (type: http)
├── skills/<name>/SKILL.md          shared by both formats
├── README.md                       user-facing: what it does, what data it sends
└── LICENSE
```

Keep `.claude-plugin/` and `.mcp.json` where they are. Claude finds them only at those paths. Moving them into a reverse-domain folder breaks the Claude install.

## Rules for every change

- **Keep both manifests in step.** `version`, `description`, `author`, `homepage`, `repository`, `license` and `keywords` match in `plugin.json` and `.claude-plugin/plugin.json`. The server URL matches in `mcp.json` and `.mcp.json`.
- **Keep `plugin.json` closed.** Only fields from the [1.0.0 manifest schema](https://agent-plugins.org/schemas/1.0.0/plugin.schema.json). Claude-only fields such as `displayName` and `privacyPolicyUrl` go only in `.claude-plugin/plugin.json`. Client-specific behavior goes under `extensions` with a reverse-domain key.
- **Match spec versions.** The `$schema` in `plugin.json` and `mcp.json` name the same Agent Plugins version.
- **Stay inside the package.** Every file the plugin uses lives under `plugins/artfct/`. No symlinks out of it, and no `../` paths.
- **Skills are flat.** Each skill is `skills/<name>/SKILL.md`, exactly one level down. Its frontmatter has a `name` equal to the directory name and a non-empty `description` ([Agent Skills spec](https://agentskills.io/specification)). Starters and references sit next to it in `assets/` and `references/`.
- **No local runtime.** The plugin runs no commands and needs no Node, Python or binaries on the person's computer. That is why `artfct-init` is not here. The only MCP server is the hosted `https://artfct.dev/mcp`.
- **Skills defer to the server.** The `artfct-{publish,search,collaborate,data}` skills mirror the server's `get_guidance` topics word for word. The `onboard` skill only drives the server's onboarding steps, so the `host` values it names must be ones the server accepts. Change the server guidance first, then the skill here.

## What clients check

A conforming client enforces the list below on this package. It is the [client conformance checklist](https://agent-plugins.org/client-implementers/conformance) for Agent Plugins 1.0.0, which is non-normative. The [specification](https://agent-plugins.org/specification) governs when wording differs. Read it as the set of ways our package can fail to load.

### Plugin loader

- Load a plugin from a directory and enforce the filesystem-resolved package boundary.
- Select locally supported manifest rules from `$schema`; do not retrieve a schema during loading.
- Validate the closed `plugin.json` schema and required `$schema` and `name` fields.
- Report and ignore unknown top-level fields.
- Ignore a non-object `extensions` field and unimplemented extension namespaces.
- Reject other fatal manifest violations before component discovery.

### Discovery and isolation

- Discover supported component types only from their fixed locations.
- Treat missing component locations as valid absence.
- Isolate invalid component types, skills, and MCP entries at their specified boundaries.
- Ignore unsupported component types and support at least one of skills or MCP servers.

### MCP support

- If MCP is implemented, support at least one of stdio and Streamable HTTP; support for both is recommended.
- Use each server entry's declared transport for the initial connection attempt.
- Validate the closed top-level document and each server entry independently.
- Resolve stdio commands as single executable tokens.
- Provide `PLUGIN_ROOT` and a dedicated persistent `PLUGIN_DATA` directory.
- Expand only the two defined placeholders and only in `args`, `env` values, and `cwd`.
- Enforce working-directory containment and remote URL/header requirements.
- Continue loading after an independent MCP server fails.

### Versioning

- Require matching Agent Plugins versions in `plugin.json` and `mcp.json`.
- Never reassign a published canonical schema identifier to different contents.
- Allow plugins to continue targeting older versions according to locally supported compatibility policy.

### What this means for us

| Client behavior | Consequence for this package |
|---|---|
| Closed manifest, fatal violations rejected | A bad `plugin.json` blocks the whole plugin, skills and MCP together. Validate it before every release. |
| Unknown top-level fields reported | Extra fields load but print warnings. Keep Claude-only fields out of `plugin.json`. |
| Fixed locations only | Skills outside `skills/<name>/` and MCP config outside `mcp.json` are invisible. |
| Invalid skills and MCP entries isolated | A broken `SKILL.md` drops only that skill, so check every skill's frontmatter rather than relying on a load test. |
| Clients may support stdio only | Our server is `streamable-http`. A stdio-only client loads the skills without the tools. Older OpenClaw builds did this. |
| No schema fetch at load time | The `$schema` URL must be the exact canonical 1.0.0 identifier, not a mirror or a `latest` alias. |
| Matching versions required | Bump `plugin.json` and `mcp.json` `$schema` together when targeting a new spec version. |
| Package boundary enforced | A symlink or path that resolves outside `plugins/artfct/` fails the load. |

## Validate before a release

```sh
# Claude format
claude plugin validate plugins/artfct
claude plugin validate .

# Agent Plugins format: both documents against the canonical 1.0.0 schemas
npx -y ajv-cli@5 validate --spec=draft2020 --strict=false \
  -s <(curl -s https://agent-plugins.org/schemas/1.0.0/plugin.schema.json) -d plugins/artfct/plugin.json
npx -y ajv-cli@5 validate --spec=draft2020 --strict=false \
  -s <(curl -s https://agent-plugins.org/schemas/1.0.0/mcp.schema.json) -d plugins/artfct/mcp.json

# Skills: name matches the directory, description is present
for f in plugins/artfct/skills/*/SKILL.md; do
  d=$(basename "$(dirname "$f")")
  grep -q "^name: $d$" "$f" && grep -q '^description: .' "$f" || echo "bad frontmatter: $f"
done
```

Then install from this checkout in at least one Agent Plugins client (Codex CLI in a temporary profile is the quickest). Confirm it lists every skill and registers the `artfct` MCP server.

## Releasing

1. Bump `version` in both manifests.
2. Update the skill list in `plugins/artfct/README.md` if skills were added or removed.
3. Merge to `main` and tag `vX.Y.Z`. The Claude marketplace reads `main`. Update any other listings that pin a version.
