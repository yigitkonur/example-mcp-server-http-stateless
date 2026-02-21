# Changelog

## 2026-02-21 - Docs rewrite and scaffold CLI re-validation

### Documentation

- Rewrote `README.md` structure with changelog-first layout.
- Recreated docs from scratch:
  - `docs/README.md`
  - `docs/V2_SDK_OVERVIEW.md`
  - `docs/CLI_SCAFFOLDER.md`
  - `docs/HTTP_STATELESS_ARCHITECTURE.md`

### Validation updates

- Re-ran scaffold CLI end-to-end:
  - `init --install`
  - generated project build
  - live MCP tool call verification

## 2026-02-21 - Major rewrite for upcoming TypeScript SDK v2

### Rewritten

- Replaced v1 `@modelcontextprotocol/sdk` architecture with v2 package split:
  - `@modelcontextprotocol/server`
  - `@modelcontextprotocol/node`
  - `@modelcontextprotocol/express`
- Replaced legacy `.tool/.resource/.prompt` usage with:
  - `registerTool`
  - `registerResource`
  - `registerPrompt`
- Replaced `McpError`/`ErrorCode` style with `ProtocolError`/`ProtocolErrorCode`.

### Added

- New starter CLI (`src/cli.ts`) with:
  - `init <project-name> [--install] [--force]`
  - `generate <tool|resource|prompt> <name> [--force]`
- Learning docs:
  - `docs/README.md`
  - `docs/V2_SDK_OVERVIEW.md`
  - `docs/CLI_SCAFFOLDER.md`
  - `docs/HTTP_STATELESS_ARCHITECTURE.md`
- End-to-end smoke test for MCP call path:
  - `scripts/smoke-http.mjs`
- SDK v2 pre-release repack script:
  - `scripts/refresh-sdk-v2.sh`
- Vendored pinned v2 artifacts:
  - `vendor/mcp-sdk-v2/*`

### Cleaned up

- Removed old oversized runtime implementation and stale v1 behavior.
- Simplified compose files and removed unused runtime env entries.
- Added CI smoke step after build/lint/typecheck/format checks.

### Validation performed

- `npm run ci` in this repository (build + typecheck + lint + format + smoke).
- Generated starter project via CLI, installed dependencies, built it, and verified a live tool call over `/mcp`.
- `mcp-cli` verification flow on both main example server and scaffolded server:
  - connect
  - inventory + schema inspect
  - valid and invalid tool calls
