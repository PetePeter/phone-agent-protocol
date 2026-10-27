# Base Protocol: Overview

**Protocol Revision:** `2026-10-27`

Every PhAP message MUST be a valid [JSON-RPC 2.0](https://www.jsonrpc.org/specification) message encoded as UTF-8 JSON. There are three message types.

## Requests

```typescript
{
  jsonrpc: "2.0";
  id: string | number;
  method: string;
  params?: { [key: string]: unknown };
}
```

- Every request carries an `id`, which is a string or an integer; a `null` value is not permitted.
- A client MUST NOT have two outstanding requests with the same `id` in one session. For the effect of reusing an `id` across sessions, see [Idempotency](utilities/idempotency.md).
- In this revision only clients send requests. Servers MUST NOT send requests.

## Responses

```typescript
{
  jsonrpc: "2.0";
  id: string | number;
  result?: { [key: string]: unknown };
  error?: { code: number; message: string; data?: unknown };
}
```

- The `id` of a response matches the request it answers.
- A response carries `result` on success or `error` on failure, never both.
- Methods with no meaningful result MUST return an empty object `{}`.
- Error codes are defined in [Errors](../errors.md).

## Notifications

```typescript
{
  jsonrpc: "2.0";
  method: "event";
  params: Event;
}
```

- The server sends every PhAP notification with `method` set to `"event"`.
- Clients MUST NOT send notifications. A server MUST ignore any notification it receives from a client.
- Every `Event` has the following common fields:

| Field | Type | Description |
|---|---|---|
| `seq` | integer ≥ 1 | Strictly increasing within one server run. |
| `ts` | string | RFC 3339 timestamp, in UTC. |
| `type` | string | The event type, for example `call.ringing`. |

The remaining fields depend on `type` and are defined with each feature.

## Batching

JSON-RPC batches MUST NOT be used. A server receiving a batch MUST respond with error `-32600` (Invalid Request).

## Method Naming

Method names use the form `<feature>.<verb>`, for example `call.dial`. Event types use the form `<feature>.<noun>` or a single word, for example `call.state` or `heartbeat`.

## Extensibility

- Receivers MUST ignore unknown object members.
- Clients MUST ignore events whose `type` they do not recognise.
- Servers MUST respond to unknown methods with `-32601` (Method not found).
