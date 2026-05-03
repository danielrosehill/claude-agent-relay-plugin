# Claude-Agent-Relay-Plugin

Direct agent-to-agent communication and coordination within a LAN. Two (or more) Claude instances on different machines exchange messages and files via a shared MCP relay server.

## Use cases

- Claude on a desktop coordinating with Claude on a home-assistant server.
- Claude on one machine asking Claude on another to investigate a problem locally (logs, config, hardware).
- Pairs of agents handing off tasks across machines without manual copy/paste.

## Trust model

LAN-only and trust-based. The relay does not authenticate users — it identifies clients by a string and an optional shared bearer token. Run it on a network you control.

## Components

- **MCP relay server** (`server/`) — Python + FastMCP, SQLite for sessions/messages, content-addressed blob store for attachments. Streamable HTTP transport. TOON used for list responses where it saves tokens vs JSON.
- **Skills** (`skills/`):
  - `setup-relay-server` — deploy the server to a host (typically a small LAN VM)
  - `connect-as-client` — register the relay as an MCP server in the local Claude install
  - `create-session` — open a named session between two clients
  - `send-message` / `send-file` — convenience wrappers
  - `tail-session` — stream new messages from a session
  - `relay-status` — health, sessions, blob storage usage
- **Client config** (`client/mcp-config.example.json`) — boilerplate the connect skill personalizes.

## Installation

```bash
claude plugins install agent-relay@danielrosehill
```

Then run the `setup-relay-server` skill on the host that will run the relay, and `connect-as-client` on each Claude machine that should join.

## License

MIT
