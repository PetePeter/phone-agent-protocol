# Errors

**Protocol Revision:** `2026-10-27`

Errors are JSON-RPC error objects of the form `{ code, message, data? }`.
- `message` MUST be the name given in the tables below.
- `data` MAY carry details. It SHOULD include `detail` (a human-readable string) wherever that is useful.

## JSON-RPC Standard Codes

| Code | Name | When |
|---|---|---|
| `-32700` | `parse_error` | Invalid JSON |
| `-32600` | `invalid_request` | Not a valid request, or a batch was sent |
| `-32601` | `method_not_found` | Unknown method |
| `-32602` | `invalid_params` | Schema violation, a method called in the wrong call state, an ambiguous contact, or an unknown handle |
| `-32603` | `internal_error` | Unexpected server failure |
| `-32001` | `unauthorized` | Bad or missing token (followed by close `4401`) |

## PhAP Codes

| Code | Name | When |
|---|---|---|
| `1001` | `not_allowed` | The target number is not on the allowlist |
| `1002` | `no_link` | The device is not connected, or the engine is stopped |
| `1003` | `no_such_call` | The `callId` is unknown or the call has ended |
| `1004` | `busy` | There is already a controller, or a conflicting operation is in progress |
| `1005` | `not_controller` | An observer invoked a ✱ method |
| `1006` | `unsupported` | The method belongs to an undeclared capability |
| `1007` | `phone_error` | The device refused the operation; `data.detail` carries the device reason |
| `1008` | `version_mismatch` | The protocol revision is not supported; `data.supported` lists the revisions the server supports |

Codes `1000`–`1999` are reserved for this specification. Implementations MAY define their own codes in the range `2000`–`2999`.
