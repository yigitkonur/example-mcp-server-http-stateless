# MCP HTTP Stateless Boilerplate (TypeScript SDK v2) + Scaffold CLI

Learning-first MCP starter focused on **HTTP stateless servers** with the official **TypeScript SDK v2 pre-release** model.

## Documentation

- Full docs hub: `docs/README.md`
- SDK v2 model and migration framing: `docs/V2_SDK_OVERVIEW.md`
- CLI usage and scaffolding workflow: `docs/CLI_SCAFFOLDER.md`
- Runtime lifecycle and stateless design notes: `docs/HTTP_STATELESS_ARCHITECTURE.md`
- Release history: `CHANGELOG.md`

## What This Repository Includes

1. A runnable stateless server reference:
   - `src/server.ts`
   - `src/mcpServer.ts`
2. A project starter / generator CLI:
   - `src/cli.ts`
3. Pinned SDK v2 pre-release tarballs for reproducibility:
   - `vendor/mcp-sdk-v2/*`

## SDK v2 Context

As of **February 21, 2026**, this repository tracks SDK v2 in pre-release/main-branch form and intentionally uses:

- `McpServer` from `@modelcontextprotocol/server`
- `NodeStreamableHTTPServerTransport` from `@modelcontextprotocol/node`
- `createMcpExpressApp` from `@modelcontextprotocol/express`
- `registerTool`, `registerResource`, `registerPrompt`
- `ProtocolError`, `ProtocolErrorCode`
- Zod v4 (`zod/v4`)

The server and generated starter projects use explicit stateless transport:

- `NodeStreamableHTTPServerTransport({ sessionIdGenerator: undefined })`

## Quick Start

```bash
git clone https://github.com/yigitkonur/example-mcp-stateless.git
cd example-mcp-stateless
npm install
npm run dev
```

Default endpoints:

- MCP: `http://127.0.0.1:1071/mcp`
- Health: `http://127.0.0.1:1071/health`

## Scaffold CLI

Build CLI first:

```bash
npm run build
```

Create a new project:

```bash
npm run cli -- init my-mcp-server --install
```

Generate stubs inside a project:

```bash
npm run create -- generate tool my_tool
npm run create -- generate resource my_resource
npm run create -- generate prompt my_prompt
```

See full command guide in `docs/CLI_SCAFFOLDER.md`.

## Included Example Primitives

Tools:

- `calculate`
- `describe_stateless_limits`

Resources:

- `boilerplate://limitations`
- `boilerplate://topic/{topic}`

Prompt:

- `design-next-tool`

## Validation

Primary checks:

```bash
npm run build
npm run check
npm run smoke
npm run ci
```

`mcp-cli` verification example:

```json
{
  "mcpServers": {
    "stateless-main": {
      "url": "http://127.0.0.1:1071/mcp"
    }
  }
}
```

```bash
MCP_NO_DAEMON=1 mcp-cli -c mcp_servers.json
MCP_NO_DAEMON=1 mcp-cli -c mcp_servers.json info stateless-main
MCP_NO_DAEMON=1 mcp-cli -c mcp_servers.json info stateless-main calculate
MCP_NO_DAEMON=1 mcp-cli -c mcp_servers.json call stateless-main calculate '{"a":8,"b":3,"op":"add","precision":2}'
MCP_NO_DAEMON=1 mcp-cli -c mcp_servers.json call stateless-main calculate '{"a":"bad","b":3,"op":"add"}'
```

For prompts/resources/templates checks, use direct JSON-RPC calls to `POST /mcp` (current `mcp-cli` is tool-centric).

## Scope and Limits

This project intentionally focuses on **HTTP stateless MCP**:

- no server-managed session continuity
- no resumability/event replay in this mode
- no in-memory multi-step workflows
- long-running workflows should use durable external systems

Additional SDK v2 constraints:

- server-side legacy SSE transport removed
- server-side auth helpers removed from SDK scope
- host header and DNS rebinding policy should be enforced by middleware/runtime setup

## Refreshing Pinned SDK Tarballs

If you have a local checkout of the official SDK main branch:

```bash
npm run refresh:sdk-v2 -- ../typescript-sdk
```

This repacks server/node/express tarballs and updates `vendor/mcp-sdk-v2/PINNED_SDK_COMMIT.txt`.

## Official SDK References

- https://github.com/modelcontextprotocol/typescript-sdk/blob/main/README.md
- https://github.com/modelcontextprotocol/typescript-sdk/blob/main/docs/server.md
- https://github.com/modelcontextprotocol/typescript-sdk/blob/main/docs/migration.md
- https://github.com/modelcontextprotocol/typescript-sdk/blob/main/docs/faq.md
- https://github.com/modelcontextprotocol/typescript-sdk/blob/main/examples/server/src/simpleStatelessStreamableHttp.ts

## License

MIT
