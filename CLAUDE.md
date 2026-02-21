# CLAUDE.md

## Project Overview

This repository is a learning-oriented **MCP HTTP stateless** boilerplate using **TypeScript SDK v2 pre-release APIs**.

It contains:

- A runnable reference server (`src/server.ts`, `src/mcpServer.ts`)
- A starter/generator CLI (`src/cli.ts`)
- Learning material (`docs/`)

## Core Technical Rules

- Transport: `NodeStreamableHTTPServerTransport`
- Mode: stateless only (`sessionIdGenerator: undefined`)
- Registration APIs: `registerTool`, `registerResource`, `registerPrompt`
- Schemas: Zod v4 (`z.object(...)`), no raw shape shortcuts
- Error model: `ProtocolError` / `ProtocolErrorCode`

## Dependency Model

SDK v2 packages are pinned as local tarballs in `vendor/mcp-sdk-v2`.
Do not replace them with `@modelcontextprotocol/sdk` v1.

Refresh command:

```bash
npm run refresh:sdk-v2 -- ../typescript-sdk
```

## Validation Commands

```bash
npm run build
npm run check
npm run smoke
```

`npm run smoke` is required for end-to-end MCP verification.
