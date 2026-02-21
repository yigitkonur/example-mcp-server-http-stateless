# Scaffold Creator CLI Guide

## Commands

Project init:

```bash
npm run build
npm run cli -- init my-mcp-server --install
```

Primitive generation in current project:

```bash
npm run create -- generate tool my_tool
npm run create -- generate resource my_resource
npm run create -- generate prompt my_prompt
```

## What `init` creates

- project skeleton (`src/server.ts`, `package.json`, `tsconfig.json`, `.gitignore`)
- vendored v2 tarballs in `vendor/mcp-sdk-v2`
- starter README and default hello tool

## Verification status

CLI was validated during this rewrite:

1. generated project created successfully
2. dependency installation completed
3. generated server built and started
4. live MCP call to `/mcp` returned the expected hello tool result

## Options

- `--install`: run `npm install` inside generated project
- `--force`: allow non-empty target directory overwrite behavior for `init` and overwrite behavior for `generate`

## Troubleshooting

- If `init` fails with missing vendor directory, run from this repository root and ensure `vendor/mcp-sdk-v2` exists.
- If generated project cannot install, check local Node version (`>=20`) and file permissions.
