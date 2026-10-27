# Changelog

All notable changes to the Phone Agent Protocol specification are recorded here. Revisions are identified by date (`protocolVersion`).

## [2026-10-27]: Draft

The initial draft revision.

- Base protocol: JSON-RPC 2.0 over WebSocket.
- Lifecycle: `hello` / `welcome`, with:
  - date-based version negotiation
  - capability negotiation
  - controller and observer roles
- Utilities: heartbeat, resumption by `seq`, idempotency of state-changing requests.
- Server features:
  - calls, including speech in and out
  - conference
  - SMS
  - contacts
  - transcripts
  - engine control
- Error codes `1001`–`1008`, plus JSON-RPC standard codes and `-32001` unauthorized.
- Security requirements:
  - allowlist enforcement on the server
  - configuration only out of band
  - handling of untrusted content and prompt injection
- An optional MCP projection, for plain LLM command-line tools (outbound only).
- A JSON Schema (draft 2020-12) for every message.
