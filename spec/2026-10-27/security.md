# Security Considerations

**Protocol Revision:** `2026-10-27`

A PhAP server can spend money and act in its owner's name: it places calls, sends messages and speaks to people. The requirements below are normative.

## 1. Server-Side Policy Enforcement

- Every outbound action MUST be checked against the allowlist by the server, before the device is contacted. Outbound actions are `call.dial`, every leg of `conference.create`, and `sms.send`.
- These checks MUST apply equally to every protocol surface the server exposes, including the [MCP projection](mcp-projection.md).
- Clients MAY pre-check, but servers MUST NOT rely on them doing so.

## 2. Configuration Is Out of Band

- The allowlist, token, fallback policy, timeouts and device selection MUST be set only through server configuration, read at startup.
- No PhAP method may read the token, and no PhAP method may change any of these settings.
- This ensures that a compromised or confused model cannot widen its own authority.

## 3. Untrusted Content and Prompt Injection

The following are **untrusted content**:
- caller speech (`call.utterance.text`, transcripts)
- SMS bodies and previews
- contact and caller names

Untrusted content is controlled by third parties and may contain text written to manipulate a language model.

- Servers MUST pass untrusted content through unchanged, and MUST NOT interpret it.
- Clients MUST present untrusted content to models in a way that marks it clearly as data, for example by quoting or delimiting it with its provenance. Clients MUST NOT merge it into system or developer instructions.
- Clients SHOULD run calls under a restricted model or policy by default. They SHOULD escalate privileges only on explicit host-side rules, never because untrusted content asked for it.

## 4. Network Exposure

- Servers SHOULD bind to loopback by default.
- On any non-loopback bind, TLS is REQUIRED.
- Tokens MUST be compared in constant time, and SHOULD be high-entropy (≥ 128 bits).

## 5. Device Command Injection

- Numbers and other parameters that are forwarded to a device MUST be validated against a strict grammar, such as E.164 or a digits-only pattern.
- This prevents injection of control sequences, for example AT command separators.

## 6. Privacy

- Transcripts, messages and contacts are personal data.
- Servers SHOULD keep them in memory, or in user-controlled storage only. They SHOULD document their retention.
- Observers receive the full event stream. Hosts SHOULD grant observer access with the same care as controller access.
