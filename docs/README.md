# Documentation Hub

This folder contains the learning and operational docs for this MCP boilerplate.

- Repository overview and quick start: `../README.md`
- Release history: `../CHANGELOG.md`

## Recommended Reading Order

1. `../README.md`  
   Use this for setup, first run, and local verification commands.
2. `V2_SDK_OVERVIEW.md`  
   Understand what changed in SDK v2 and which v2 patterns this project enforces.
3. `HTTP_STATELESS_ARCHITECTURE.md`  
   Learn the request lifecycle, stateless guarantees, and trade-offs.
4. `CLI_SCAFFOLDER.md`  
   Generate starter projects and primitive stubs with the built-in CLI.

## Document Index

### `V2_SDK_OVERVIEW.md`

- v2 package model and API shifts used in this repo
- v1 concepts intentionally removed
- practical migration and implementation notes

### `HTTP_STATELESS_ARCHITECTURE.md`

- per-request server/transport lifecycle
- endpoint behavior and cleanup model
- stateless trade-offs and mitigation guidance

### `CLI_SCAFFOLDER.md`

- CLI command reference (`init`, `generate`)
- generated project layout
- validation and troubleshooting workflow

## Scope of This Documentation

- MCP TypeScript SDK v2-oriented implementation
- HTTP stateless transport only
- starter CLI and generated project workflow

Stateful/SSE/server-side OAuth flows are intentionally out of scope in this repository.
