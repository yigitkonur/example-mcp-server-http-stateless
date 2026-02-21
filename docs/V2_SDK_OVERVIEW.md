# MCP TypeScript SDK v2 Overview for This Boilerplate

## Why v2 matters here

This boilerplate adopts the v2 shape now so teams can learn and build around the future model instead of v1 compatibility layers.

Key v2-facing differences used in this project:

- package split (`@modelcontextprotocol/server`, `@modelcontextprotocol/node`, `@modelcontextprotocol/express`)
- registration API (`registerTool`, `registerResource`, `registerPrompt`)
- protocol error model (`ProtocolError`, `ProtocolErrorCode`)
- request context model (`ctx.mcpReq.*`)
- Zod v4 schema expectations

## v2 pre-release reality

As of **February 21, 2026**, this repository treats SDK v2 as pre-release/main-branch workflow context and pins vendor tarballs under `vendor/mcp-sdk-v2`.

Why:

- stable reproducibility for this learning boilerplate
- fast local bootstrap for generated starter projects
- commit-level traceability via `PINNED_SDK_COMMIT.txt`

## What was removed from v1-style patterns

- no `@modelcontextprotocol/sdk` package usage
- no `.tool()`, `.resource()`, `.prompt()` calls
- no `McpError` / `ErrorCode` usage
- no server-side SSE legacy transport path

## Practical recommendation

Treat this repository as a v2 learning baseline and keep production risk controls explicit while v2 APIs continue evolving.
