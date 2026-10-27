# Authorization

**Protocol Revision:** `2026-10-27`

## Bearer Token

This revision defines a single mechanism: a shared bearer token, presented in `hello.params.token`.

- The server MUST compare tokens in constant time.
- If the token is missing or wrong, the server MUST return error `-32001` with message `"unauthorized"`, and then close with code `4401`.
- The token MUST be configured on the server out of band (see [Configuration](../configuration.md)). It MUST NOT be obtainable or changeable through PhAP.
- Clients SHOULD NOT log the token.

## Transport Security

- A token sent over a non-loopback connection without TLS is exposed to the network. For that reason, servers bound to a non-loopback address MUST require `wss://` (see [Transports](transports.md)).

## Authorization Scope

- Holding a valid token authorizes a session to request either role. Role exclusivity is governed by [Lifecycle](lifecycle.md).
- Every outbound action remains subject to the server's allowlist, whatever the session's role or token. See [Security](../security.md).

## Future Mechanisms

Later revisions MAY add other mechanisms, such as OAuth 2.1 or mutual TLS. Servers MAY support them as extensions, but MUST continue to support the bearer token for this revision.
