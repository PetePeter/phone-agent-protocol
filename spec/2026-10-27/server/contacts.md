# Contacts

**Protocol Revision:** `2026-10-27`

## Capabilities

```json
{ "capabilities": { "contacts": {} } }
```

## Methods

### `contacts.search`

```json
{ "jsonrpc": "2.0", "id": "k1", "method": "contacts.search", "params": { "query": "oct" } }
```
```json
{ "jsonrpc": "2.0", "id": "k1", "result": { "matches": [
  { "name": "Octavio", "numbers": [ { "number": "+61491570157", "label": "mobile" } ] }
] } }
```

- Matching is case-insensitive and on substrings.
- An empty `query` MAY return every contact, up to a server-defined limit.
- The server MUST NOT guess between candidates. Every match MUST be returned, and when a name matches more than one contact, the client chooses.
- Being in the contacts list does **not** put a number on the allowlist.

## Security Considerations

- Contact names are **untrusted content**, because they may have been entered by third parties through synchronisation.
