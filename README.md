stateless HTTP transport for MCP. every request gets a fresh server instance — no sessions, no state, no coordination between replicas. deploy it as a serverless function, behind a load balancer, or scale horizontally without thinking about it.

uses a calculator domain as the example, but the point is the architecture, not the math.

```bash
npm run dev
# listening on http://localhost:1071
```

[![node](https://img.shields.io/badge/node-20+-93450a.svg?style=flat-square)](https://nodejs.org/)
[![typescript](https://img.shields.io/badge/typescript-5.7-93450a.svg?style=flat-square)](https://www.typescriptlang.org/)
[![license](https://img.shields.io/badge/license-MIT-grey.svg?style=flat-square)](https://opensource.org/licenses/MIT)

> **series:** [STDIO](https://github.com/yigitkonur/example-mcp-server-stdio) | [stateful HTTP](https://github.com/yigitkonur/example-mcp-server-streamable-http) | **stateless HTTP** | [SSE](https://github.com/yigitkonur/example-mcp-server-sse)

---

## what it does

- **fresh instance per request** — new `McpServer` + `StreamableHTTPServerTransport` on every call, torn down when the response closes
- **no session management** — `sessionIdGenerator: undefined` tells the SDK to run stateless
- **DNS rebinding protection** — delegated to the SDK transport, not hand-rolled middleware
- **structured JSON logging** — immutable context-chaining logger, per-request `requestId` correlation
- **prometheus metrics** — request duration histograms (p50/p95/p99), per-tool execution time, memory gauges
- **rate limiting** — per-IP via `express-rate-limit`, configurable window and max
- **CORS** — preflight cached 24h, MCP protocol headers whitelisted
- **graceful shutdown** — SIGTERM/SIGINT handlers, clean server close
- **progress streaming** — tools can emit `notifications/progress` over SSE via `GET /mcp`

## endpoints

| method | path | what it does |
|:---|:---|:---|
| `POST` | `/mcp` | MCP JSON-RPC requests (tools, resources, prompts) |
| `GET` | `/mcp` | SSE stream for progress notifications |
| `DELETE` | `/mcp` | returns 405 — sessions don't exist here |
| `GET` | `/health` | basic health check |
| `GET` | `/health/detailed` | system info, config, scaling characteristics |
| `GET` | `/metrics` | prometheus text format |

## tools

| name | parameters | description |
|:---|:---|:---|
| `calculate` | `a`, `b`, `op` (add/subtract/multiply/divide), `stream?`, `precision?` | arithmetic with optional SSE progress notifications |
| `demo_progress` | none | sends 5 progress events at 200ms intervals |

set `SAMPLE_TOOL_NAME` env var to register a dynamic echo tool at runtime.

## resources

| URI | description |
|:---|:---|
| `calculator://constants` | pi and e |
| `calculator://history/{id}` | intentionally throws — demonstrates stateless incompatibility |
| `calculator://stats` | uptime, timestamp, pattern |
| `formulas://library` | 10 math formulas with category and description |
| `request://current` | per-request metadata: request ID, process info, memory usage |

## prompts

| name | parameters | description |
|:---|:---|:---|
| `explain-calculation` | `calculation`, `level?` | step-by-step explanation at basic/intermediate/advanced |
| `generate-problems` | `topic`, `difficulty?`, `count?` | practice problems with answer key |
| `calculator-tutor` | `topic?`, `studentLevel?` | tutoring persona that uses the `calculate` tool |

## install

```bash
git clone https://github.com/yigitkonur/example-mcp-server-http-stateless.git
cd example-mcp-server-http-stateless
npm install
```

### run

```bash
npm run dev                 # hot-reload via tsx
npm run build && npm start  # compiled production
```

### docker

```bash
npm run docker:build && npm run docker:up   # production
npm run docker:dev                          # dev with volume mounts
npm run docker:logs                         # tail logs
npm run docker:down                         # stop
```

multi-stage alpine build — dev deps and source excluded from production image.

### test with MCP inspector

```bash
npx @modelcontextprotocol/inspector --cli http://localhost:1071/mcp
```

### manual curl

```bash
curl -X POST http://localhost:1071/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","method":"tools/call","params":{"name":"calculate","arguments":{"a":15,"b":7,"op":"add"}},"id":1}'
```

## configuration

all env vars, no config files.

| variable | default | description |
|:---|:---|:---|
| `PORT` | `1071` | listen port |
| `CORS_ORIGIN` | `*` | allowed origin. restrict in production |
| `LOG_LEVEL` | `info` | debug, info, warn, error |
| `RATE_LIMIT_MAX` | `1000` | max requests per IP per window |
| `RATE_LIMIT_WINDOW` | `900000` | rate limit window in ms (15 min) |
| `ENABLE_METRICS` | `true` | set to `false` to disable |
| `SAMPLE_TOOL_NAME` | — | if set, registers a dynamic echo tool with this name |

## project structure

```
src/
  types.ts    — zod schemas, constants, type definitions. no logic, no side effects
  server.ts   — express app, MCP server factory, route handlers, lifecycle
```

two files. `types.ts` is the stable contract, `server.ts` is all runtime behavior. the separation is intentional.

## the stateless pattern

the core of this repo is one function. for every HTTP request:

1. create a new `McpServer` instance
2. create a new `StreamableHTTPServerTransport` with no session ID generator
3. connect them
4. let the SDK handle the JSON-RPC request
5. close both when the response stream ends

no global state, no session map, no cleanup jobs. the trade-off is that `calculator://history/{id}` deliberately throws — some resources are fundamentally incompatible with stateless operation, and the code demonstrates that explicitly.

## error handling

three layers:

- **middleware** — rate limiter and size guard return JSON-RPC errors before reaching the handler
- **tool-level** — `McpError` with proper error codes (divide by zero, invalid params). SDK serializes these as JSON-RPC errors
- **safety net** — catch-all in the request handler. logs the stack trace, returns generic 500. never leaks internals to the client

## license

MIT
