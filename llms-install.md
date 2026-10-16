# Installing the Nerdit MCP server

Nerdit is a remote MCP server with OAuth. Nothing needs to be installed locally.

1. Add a remote MCP server named `nerdit` with the URL `https://mcp.nerdit.ai/mcp` (Streamable HTTP). No API key and no headers.
2. On first use the client opens Nerdit's sign-in in the browser. The user signs in (or creates a free account at https://app.nerdit.ai), chooses which projects or machines the agent may reach, and approves.
3. If the user has no machine yet, they can link one with `curl -fsSL get.nerdit.ai | sh` or start a Pro box from https://app.nerdit.ai/box.
4. Check the connection by calling `nerdit_list_nodes`.

The self-hosted engine (github.com/nerdit-ai/nerdit) also ships a local stdio server: `pip install nerdit`, then run `nerdit mcp`.
