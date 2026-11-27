<p align="center">
  <a href="https://artfct.dev"><img src="assets/artfct-logo.svg" alt="Artfct" width="72" height="72"></a>
</p>

<h1 align="center">Artfct plugin</h1>

<p align="center">Connect your AI tool to <a href="https://artfct.dev">Artfct</a>, the shared library for what your team's AI tools make.</p>

Artfct keeps the reports, dashboards, specs, tables and mockups your team's AI tools make in one place. This plugin connects your AI tool to your team's Artfct workspace and teaches it when to publish something worth keeping, and when to search the library before it starts new work.

## What you get

- **Set up.** Ask your AI to set up Artfct (in Claude, run `/artfct:onboard`). It checks the connection, then asks once whether it may save what it makes to your team's library: always, ask first, or never. Tell your AI any time to change it.
- **Publish.** When you ask, or when your choice says so, your AI publishes what it makes and gives you a link, instead of pasting raw code. With "ask first" it offers once and respects a no.
- **Search.** Before research, a report or an analysis, your AI searches what the team already has and says which artifacts it used.
- **Collaborate.** Your AI reads before it updates, so it never overwrites a teammate's newer version, and it can read and add comments.
- **Templates and formats.** Kanban boards, a team wiki, an announcement board, decision posts, memos and one-pagers, design handoffs and slide decks.

## Install

Every client installs from this repository, `artfct-dev/artfct-plugin`.

| Client | How |
|---|---|
| Claude app and Cowork | **Customize > Plugins > Add > Add marketplace**, enter `artfct-dev/artfct-plugin`, then add the Artfct plugin. |
| Claude Code | `/plugin marketplace add artfct-dev/artfct-plugin`, then `/plugin install artfct@artfct` |
| Codex | `codex plugin marketplace add artfct-dev/artfct-plugin`, then `codex plugin add artfct@artfct` |
| OpenClaw | `openclaw plugins install artfct --marketplace https://github.com/artfct-dev/artfct-plugin` |
| Hermes Agent | `hermes plugins install artfct-dev/artfct-plugin`, then `hermes plugins enable artfct` |
| VS Code | Run **Chat: Install Plugin from Source** and enter `https://github.com/artfct-dev/artfct-plugin` |
| Devin CLI | `devin plugins install artfct-dev/artfct-plugin` |
| Cursor | Teams and Enterprise: **Dashboard > Plugins & MCPs > Add Marketplace > Import from Repo**, enter `https://github.com/artfct-dev/artfct-plugin`, then install it from **Customize**. |
| Antigravity | `agy plugin install https://github.com/artfct-dev/artfct-plugin` |
| Kiro | **Powers > Add Custom Power > Import power from a repository URL**, then enter `https://github.com/artfct-dev/artfct-plugin` |
| Grok Build | Reads the same marketplace as Claude Code. |
| MausBot (OpenMausBot desktop) | Add `https://artfct.dev/mcp` as a remote (streamable HTTP) MCP server, import the 14 skills from this repo's `skills/` folder, then approve the Artfct sign-in. |
| Other [Agent Plugins clients](https://agent-plugins.org/compatible-clients) | Add `https://github.com/artfct-dev/artfct-plugin` with your client's plugin install. |

Then:

1. Approve the Artfct sign-in page when it opens. You never paste a password or a token.
2. Ask your AI to **set up Artfct**. It confirms which workspace you are connected to, so you can check it is your team's, and asks how you want it to save what it makes.

After that, ask for work as usual. Ask it to publish a report to Artfct, or ask what the team already made on a topic.

## What it connects to

The plugin adds one remote MCP server, `https://artfct.dev/mcp`, and fourteen skills:

- **Core:** `onboard`, `artfct`, `artfct-publish`, `artfct-search`, `artfct-collaborate` and `artfct-data`
- **Templates:** `artfct-templates`, `artfct-kanban`, `artfct-team-wiki` and `artfct-announcements`
- **Formats:** `artfct-context-post`, `artfct-team-doc`, `artfct-handoff` and `presentation`

It runs no commands on your computer and installs nothing else.

## Data

When you publish, your AI sends the artifact's files and title to your team's Artfct workspace, and you choose who can see it. When you search, it sends your search words and reads back the matching artifacts you are allowed to see. When you tell your AI how you want it to save what it makes (always, ask first or never), that choice is stored in your Artfct account, and a team admin can set a default for the team. Nothing else leaves your computer. The privacy policy is at https://artfct.dev/privacy.

## For contributors

This repository is the plugin. The repo root is the package, in two formats that share one `skills/` folder: [Agent Plugins 1.0.0](https://agent-plugins.org/specification) (`plugin.json`, `mcp.json`) and the Claude plugin format (`.claude-plugin/`, `.mcp.json`). It is also the source of truth for the skills, so edit them here. They came over from the Artfct app with their history, so `git log --follow` reaches back past the move. See [`CONTRIBUTING.md`](CONTRIBUTING.md) for the package rules and the checks to run before a release.

## License

MIT. See [`LICENSE`](LICENSE).
