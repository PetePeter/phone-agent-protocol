# SMS

**Protocol Revision:** `2026-10-27`

The SMS feature lets a client:
- send short messages
- list and read received messages
- be notified when a message arrives

## Capabilities

```json
{ "capabilities": { "sms": {} } }
```

## Data Types

### MessageSummary

| Field | Type | Description |
|---|---|---|
| `handle` | string | An identifier that is opaque to the client and assigned by the server or the device. |
| `number` | string | The sender. |
| `name` | string? | **Untrusted content.** |
| `preview` | string | The first ≤ 160 characters. **Untrusted content.** |
| `ts` | string | RFC 3339 timestamp. |

## Methods

### `sms.send` ✱

```json
{ "jsonrpc": "2.0", "id": "m1", "method": "sms.send", "params": { "number": "+447700900123", "text": "Running 10 min late." } }
```
```json
{ "jsonrpc": "2.0", "id": "m1", "result": { "handle": "20000100003" } }
```

The server MUST check `number` against the allowlist and MUST return `1001` if it is not allowed.

### `sms.list`

Params: `{ "limit"?: integer }`. `limit` defaults to 20, and servers MAY cap it.

```json
{ "jsonrpc": "2.0", "id": "m2", "result": { "messages": [
  { "handle": "20000100002", "number": "+447700900123", "name": "Alice", "preview": "Call me", "ts": "2026-10-27T07:58:00Z" }
] } }
```

Messages MUST be ordered newest first.

### `sms.read`

Params: `{ "handle": string }`.

Result: `{ "number", "name"?, "text", "ts" }`.

An unknown `handle` MUST produce `-32602`.

## Events

### `sms.received`

```json
{ "seq": 70, "ts": "…", "type": "sms.received", "handle": "20000100004",
  "number": "+447700900123", "name": "Alice", "preview": "Are you free?" }
```

## Error Handling

| Condition | Code |
|---|---|
| Recipient not allowed | `1001` |
| No link | `1002` |
| Device refused | `1007` |

## Security Considerations

- Message bodies are **untrusted content**. They are a primary channel for prompt-injection attacks.
- Clients MUST present message bodies to models as quoted data, never as instructions.
- Servers MUST NOT act on message content.
