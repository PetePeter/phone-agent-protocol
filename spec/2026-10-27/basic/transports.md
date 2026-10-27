# Transports

**Protocol Revision:** `2026-10-27`

## WebSocket

PhAP defines a single standard transport: WebSocket, as specified in [RFC 6455](https://www.rfc-editor.org/rfc/rfc6455).

### Endpoint

- The server MUST accept connections at a single URI, for example `ws://127.0.0.1:7781/phap`.
- The client SHOULD request the subprotocol `phap` in `Sec-WebSocket-Protocol`. If the client requested it, the server MUST echo it back.

### Framing

- Each WebSocket **text** frame MUST contain exactly one JSON-RPC message.
- Binary frames MUST NOT be used in this revision. On receiving a binary frame, the receiver MUST close the connection with code `1003`.
- Messages MUST NOT be split across frames by the application. Protocol-level fragmentation is permitted.

### Binding and TLS

- By default the server SHOULD bind to a loopback address only.
- If a server binds to any non-loopback address, it MUST use TLS (`wss://`).

### Close Codes

| Code | Meaning | Sent by |
|---|---|---|
| `1000` | Normal closure | either party |
| `1003` | Unsupported data (binary frame) | either party |
| `4400` | First message was not `hello`, or `hello` was malformed | server |
| `4401` | Authentication failed | server |
| `4409` | Controller already present and the session requested the controller role (see [Lifecycle](lifecycle.md)) | server |
| `4426` | Protocol version not supported | server |

Before closing with a `44xx` code, the server SHOULD send the error response to the offending request.

### Liveness

Liveness is provided at the application layer by [Heartbeat](utilities/heartbeat.md). Implementations MAY also use WebSocket ping/pong, but MUST NOT rely on it alone.

## Custom Transports

Implementations MAY carry PhAP over other bidirectional, message-oriented transports. Any such transport MUST preserve:
- message boundaries
- ordering
- the lifecycle defined in this specification
