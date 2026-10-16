# Nerdit for Claude

Lets Claude deploy, run and share the apps you build, on your own machines or on a Nerdit box.
The plugin bundles the Nerdit connector (`https://mcp.nerdit.ai/mcp`, the same server as the
[Nerdit connector](https://claude.ai/directory/nerdit)) with four skills.

## Install

In Claude Code:

```
/plugin marketplace add nerdit-ai/nerdit-mcp
/plugin install nerdit@nerdit
```

Then run `/mcp` and sign in to Nerdit. You choose which projects or machines Claude may reach,
and you can disconnect it at any time from the Agents page at https://app.nerdit.ai.

You need a free Nerdit account and a machine you link (`curl -fsSL get.nerdit.ai | sh`, free, up
to five) or a Pro box.

## Skills

| Skill | Ask Claude |
|---|---|
| `deploy` | "Deploy this repo to my Nerdit machine." |
| `diagnose` | "My app is down, find out why." |
| `custom-domain` | "Serve my app at app.example.com." |
| `share-app` | "Let alice@acme.com open this app." |

Secrets never go through the chat: when an app needs one, Claude asks you to run
`nerdit vars set <project> --secret --prompt KEY` yourself.

## Other clients

- **Claude (web and desktop):** add the [Nerdit connector](https://claude.ai/directory/nerdit).
- **Cursor:** this repo is also a Cursor plugin (`.cursor-plugin/`, `mcp.json`).
- **Any MCP client:** add a remote server at `https://mcp.nerdit.ai/mcp` (Streamable HTTP,
  OAuth, no API key). See [llms-install.md](llms-install.md).
- **Local engine:** the self-hosted engine ([nerdit-ai/nerdit](https://github.com/nerdit-ai/nerdit))
  ships a stdio server: `pip install nerdit`, then `nerdit mcp`.

This repo holds distribution files only: the plugin manifests, skills and directory metadata
(`server.json` for the official MCP registry, `lhm.plugin.json` for LobeHub).

Docs: https://docs.nerdit.ai · Privacy: https://nerdit.ai/privacy · Support: contact@nerdit.ai
