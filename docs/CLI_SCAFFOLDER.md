# CLI Scaffolder Guide

Back to docs hub: `docs/README.md`  
Back to repository guide: `../README.md`

## Purpose

The built-in CLI helps you:

1. bootstrap a new MCP HTTP stateless project
2. generate tool/resource/prompt stub files in existing projects

Binary entrypoint (after build):

- `mcp-stateless-starter` -> `dist/cli.js`

## Command Reference

### Initialize a starter project

```bash
npm run build
npm run cli -- init <project-name> [--install] [--force]
```

Example:

```bash
npm run cli -- init my-mcp-server --install
```

### Generate primitive stubs

```bash
npm run create -- generate <tool|resource|prompt> <name> [--force]
```

Examples:

```bash
npm run create -- generate tool invoice_total
npm run create -- generate resource policy_docs
npm run create -- generate prompt onboarding_plan
```

## What `init` Produces

- starter server file: `src/server.ts`
- base project files: `package.json`, `tsconfig.json`, `.gitignore`, `README.md`
- pinned SDK v2 vendor artifacts in `vendor/mcp-sdk-v2/`
- default sample tool (`hello`)

Example structure:

```text
my-mcp-server/
  .gitignore
  README.md
  package.json
  tsconfig.json
  src/server.ts
  vendor/mcp-sdk-v2/
```

## Option Semantics

- `--install`  
  Runs `npm install` in the generated project after files are created.
- `--force`  
  Allows writing into non-empty directories (`init`) and overwriting existing generated files (`generate`).

## Validation Workflow (Recommended)

After scaffolding:

```bash
cd <project-name>
npm run build
npm run start
```

Then verify with `mcp-cli`:

```json
{
  "mcpServers": {
    "my-server": {
      "url": "http://127.0.0.1:1071/mcp"
    }
  }
}
```

```bash
MCP_NO_DAEMON=1 mcp-cli -c mcp_servers.json info my-server
MCP_NO_DAEMON=1 mcp-cli -c mcp_servers.json call my-server hello '{"name":"World"}'
```

## Troubleshooting

- Missing vendor directory during `init`:
  - run CLI from this repository root
  - verify `vendor/mcp-sdk-v2/` exists
- Install/build failures in generated project:
  - verify Node.js `>=20`
  - verify file permissions in target directory
- Unexpected stale behavior during tool calls:
  - use `MCP_NO_DAEMON=1` when testing after rebuilds
