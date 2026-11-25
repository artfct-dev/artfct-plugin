# Artfct plugin

Connect your AI tool to [Artfct](https://artfct.dev), the shared library for what your team's AI tools make.

This repository is the Artfct plugin and its marketplace. The plugin lives in `plugins/artfct`; see its [README](plugins/artfct/README.md) for what it does and what data it sends.

## Claude marketplace

In the Claude app or Cowork, open **Customize > Plugins > Add > Add marketplace** and enter `artfct-dev/artfct-plugin`. Then add the Artfct plugin.

In Claude Code, run `/plugin marketplace add artfct-dev/artfct-plugin`, then `/plugin install artfct@artfct`. Grok Build reads the same marketplace.

Approve the Artfct sign-in page when it opens.

## Agent Plugins clients

Clone this repository and use `plugins/artfct` as the Agent Plugins 1.0 package directory. Follow the installation instructions for your client and surface in the [official compatibility list](https://agent-plugins.org/compatible-clients). The package includes ten skills and the Artfct Streamable HTTP MCP server.

Approve Artfct sign-in through your client, then ask your agent to **Set up Artfct**. Confirm the workspace it reports and choose when the agent may save work to Artfct.

## Where the skills come from

The skills in `plugins/artfct/skills` are copied from the Artfct app, which owns them. Do not edit them here; changes would be overwritten by the next sync.

## License

MIT. See [`plugins/artfct/LICENSE`](plugins/artfct/LICENSE).
