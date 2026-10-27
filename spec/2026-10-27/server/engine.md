# Engine

**Protocol Revision:** `2026-10-27`

The engine is the server's connection to the device, together with its voice pipeline. This feature is always available.

## Methods

| Method | ✱ | Params | Result |
|---|---|---|---|
| `status` | | `{}` | `{ "state": State }`, the same structure as `welcome.state` |
| `engine.start` | ✱ | `{}` | `{ "state": State }` |
| `engine.stop` | ✱ | `{}` | `{ "state": State }` |

- `engine.stop` MUST end every call that is not yet `ended`, with `reason: "local"`.
- `engine.stop` MUST NOT close any session.
- While the engine is stopped, call, SMS and contacts methods MUST return `1002`.

## Events

### `engine`

```json
{ "seq": 3, "ts": "…", "type": "engine", "status": "ready" }
```

- `status` is one of `"starting"`, `"ready"`, `"stopped"` or `"error"`.
- `detail` (a string) SHOULD accompany `"error"`.

### `link`

```json
{ "seq": 4, "ts": "…", "type": "link", "status": "up", "phone": "iPhone (A4:C3:…)" }
```

- If the link goes `down` during a call, the server MUST emit `call.state` with `ended` and `reason: "link_lost"` for every call that has not yet ended.
