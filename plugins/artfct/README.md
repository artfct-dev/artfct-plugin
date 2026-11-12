# Artfct

Artfct is the shared library for what your team's AI tools make: reports, dashboards, specs, tables and mockups. This plugin connects your AI tool to your team's Artfct workspace and teaches it when to publish something worth keeping, and when to search the library before it starts new work.

## What you get

- **Publish.** When your AI makes something worth keeping, it publishes it to your team's library and gives you a link, instead of pasting raw code.
- **Search.** Before research, a report or an analysis, your AI searches what the team already has and says which artifacts it used.
- **Collaborate.** Your AI reads before it updates, so it never overwrites a teammate's newer version, and it can read and add comments.

## Use it

1. Add the plugin. In the Claude app or Cowork, use **Customize > Plugins** and search for Artfct. In Claude Code, run `/plugin install artfct --marketplace artfct-dev/artfct-plugin`.
2. Approve the Artfct sign-in page that opens in your browser. You never paste a password or a token.
3. Ask your AI "Which Artfct workspace am I connected to?" to confirm it is your team's.

Then ask for work as usual. Ask it to publish a report to Artfct, or ask what the team already made on a topic.

## What it connects to

The plugin adds one remote server, `https://artfct.dev/mcp`, and five skills: `artfct`, `artfct-publish`, `artfct-search`, `artfct-collaborate` and `artfct-data`. It runs no commands on your computer and installs nothing else.

## Data

When you publish, your AI sends the artifact's files and title to your team's Artfct workspace, and you choose who can see it. When you search, it sends your search words and reads back the matching artifacts you are allowed to see. Nothing else leaves your computer. The privacy policy is at https://artfct.dev/privacy.

## License

MIT. See `LICENSE`.
