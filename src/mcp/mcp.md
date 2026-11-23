# MCP (Model Context Protocol)

## What MCP Is
- An open protocol that lets any harness use tools (and resources and prompts) provided by an external server
- Messages are JSON-RPC
- Spec: [modelcontextprotocol.io](https://modelcontextprotocol.io/specification)

## Transports
- **stdio**: the harness starts the server as a local process
- **Streamable HTTP**: a remote server, often with OAuth

## Finding and Registering Servers
- Official registry: [registry.modelcontextprotocol.io](https://registry.modelcontextprotocol.io)
- Pi: `pi mcp add …`, `/mcp`, `~/.pi/agent/mcp.json` / `.pi/mcp.json`
- opencode: MCP servers in `opencode.json(c)`
- Hermes Agent: `hermes mcp`

## Writing a Minimal MCP Server
- Official SDKs (TypeScript, Python and others)
- Expose one tool, register it in a harness, verify the model can call it

## Security
- **Tool poisoning**: malicious instructions hidden in a tool's description, which the model reads ([Invariant Labs, 2025](https://invariantlabs.ai/blog/mcp-security-notification-tool-poisoning-attacks))
- Over-broad credentials: give each server the narrowest token that works
- A local MCP server runs with your user's permissions. Install only servers you trust
- Combining servers can create the "lethal trifecta" (see [Quality, Safety, and Evaluation](../quality-safety-and-evaluation/quality-safety-and-evaluation.md))
- Token cost: every connected server's tool schemas sit in context
