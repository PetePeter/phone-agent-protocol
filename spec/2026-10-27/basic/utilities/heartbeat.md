# Heartbeat

**Protocol Revision:** `2026-10-27`

The server MUST emit a `heartbeat` event on every session at least once every `heartbeatInterval` seconds (default `10`), whether or not other events have been sent.

```json
{ "jsonrpc": "2.0", "method": "event", "params": { "seq": 112, "ts": "2026-10-27T08:00:10Z", "type": "heartbeat" } }
```

- Heartbeats consume `seq` values and are stored in the event log like any other event.
- During [Resumption](resumption.md), a server MAY omit heartbeats from the replay. Omitting them produces gaps in `seq`, and such gaps MUST NOT be treated as data loss.
- If a client receives no event for `3 × heartbeatInterval` seconds, it SHOULD consider the session dead, close it, and reconnect with `resumeFrom`.
- The server SHOULD report its interval in `welcome.capabilities` under the key `heartbeat`, for example `"heartbeat": {"interval": 10}`.
