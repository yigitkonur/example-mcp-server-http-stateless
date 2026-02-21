# HTTP Stateless Architecture

Back to docs hub: `docs/README.md`  
Back to repository guide: `../README.md`

## Design Goal

Provide a clear, production-friendly baseline for **HTTP stateless MCP** where each request is isolated and independently executable.

## Request Lifecycle (`POST /mcp`)

For every incoming MCP request:

1. create a fresh `McpServer` instance
2. create a fresh `NodeStreamableHTTPServerTransport` with `sessionIdGenerator: undefined`
3. connect server and transport
4. process request through `transport.handleRequest(...)`
5. close both transport and server on completion/connection close

This ensures no request-to-request in-memory coupling.

## Endpoint Contract

- `POST /mcp`
  - MCP JSON-RPC request handling
- `GET /mcp`
  - explicit `405` (method not allowed)
- `DELETE /mcp`
  - explicit `405` (method not allowed)
- `GET /health`
  - service liveness and mode metadata

## Why This Stateless Pattern

- no session affinity requirement for scaling
- predictable isolation and cleanup
- easier deployment behind load balancers and serverless edges

## Trade-offs and Mitigations

- No resumability/replay in stateless mode
  - use stateful mode + durable event store if resumability is required
- No in-memory multi-step workflows
  - externalize workflow state to DB/queue/task system
- Long-running tasks are not request-bound
  - queue async work and return progress/status via external task model

## Hardening Considerations

- Keep rate limiting on `/mcp`
- Keep CORS policy explicit per environment
- Keep host-header/DNS-rebinding policy explicit at middleware/runtime layer
- Keep error responses sanitized for production

## Current Typing Constraint

`exactOptionalPropertyTypes` remains disabled in this repository due current SDK v2 alpha typing friction around optional transport handler fields.

Revisit this flag as upstream typings stabilize.
