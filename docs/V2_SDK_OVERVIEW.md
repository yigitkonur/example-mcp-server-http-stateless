# SDK v2 Overview (This Boilerplate)

Back to docs hub: `docs/README.md`  
Back to repository guide: `../README.md`

## Purpose

This repository is a **v2-first MCP learning baseline**.  
It is designed to teach and scaffold against the upcoming TypeScript SDK v2 model, not v1 compatibility patterns.

## v2 Model Used Here

Core packages:

- `@modelcontextprotocol/server`
- `@modelcontextprotocol/node`
- `@modelcontextprotocol/express`

Core server APIs:

- `registerTool`
- `registerPrompt`
- `registerResource`
- `NodeStreamableHTTPServerTransport({ sessionIdGenerator: undefined })` for explicit stateless mode

Core error model:

- `ProtocolError`
- `ProtocolErrorCode`

Core schema model:

- Zod v4 with explicit `z.object(...)` wrappers for tool/prompt schemas

## v1 Concepts Intentionally Removed

- no `@modelcontextprotocol/sdk` package imports
- no `.tool()`, `.prompt()`, `.resource()` variadic APIs
- no `McpError` / `ErrorCode` usage
- no server-side legacy SSE transport path

## Pre-release Reality and Pinning Strategy

As of **February 21, 2026**, v2 is treated as pre-release/main-branch workflow context.  
To keep this starter stable and reproducible, the repository vendors tarballs under:

- `vendor/mcp-sdk-v2/`

The pinned upstream commit is tracked in:

- `vendor/mcp-sdk-v2/PINNED_SDK_COMMIT.txt`

Refresh command:

```bash
npm run refresh:sdk-v2 -- ../typescript-sdk
```

## Practical Implementation Notes

- Keep transport config explicit for stateless mode.
- Keep request handling per-request (new server + new transport each POST).
- Keep docs and examples synchronized with official SDK migration/server docs.
- Re-run `npm run ci` after SDK tarball refresh or API-facing edits.
