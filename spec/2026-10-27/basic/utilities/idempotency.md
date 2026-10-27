# Idempotency

**Protocol Revision:** `2026-10-27`

If a connection drops after a client sent a ✱ (state-changing) request but before the response arrived, the client cannot tell whether the action happened. Idempotency makes it safe to retry.

## Requirements

- For every ✱ request, the server MUST remember the request `id` and its response for at least **60 seconds** after sending the response.
- This record MUST be kept per **client identity**, which is `clientInfo.name` together with the token. It MUST survive the end of the session.
- If the server receives a ✱ request whose `id` matches a remembered one from the same client identity, it MUST:
  - **not** perform the action again
  - return the remembered response unchanged, whether it was a result or an error
- If the matching `id` arrives while the original request is still in progress, the server MUST wait for it to finish and then return the same response.
- A client that retries MUST reuse the original `id` and the original `params`. If the `params` differ, the server MAY return `-32602` (Invalid params).
- Clients SHOULD use globally unique ids, for example UUIDs, for ✱ requests.

Methods that are not ✱ are naturally safe to repeat, and the server MAY skip this tracking for them.
