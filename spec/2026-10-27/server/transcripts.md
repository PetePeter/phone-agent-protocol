# Transcripts

**Protocol Revision:** `2026-10-27`

Transcripts give access to the utterances of a call after the fact. This feature is always available. Its content depends on `voice.stt`.

## Methods

### `transcript.get`

```json
{ "jsonrpc": "2.0", "id": "t1", "method": "transcript.get", "params": { "callId": "c7" } }
```
```json
{ "jsonrpc": "2.0", "id": "t1", "result": { "callId": "c7", "utterances": [
  { "speaker": "remote", "text": "Hi, is Sam there?", "startMs": 1200, "endMs": 2650 },
  { "speaker": "local",  "text": "Sam's out, can I take a message?", "startMs": 3100, "endMs": 5200 }
] } }
```

- `speaker` is `"remote"` for recognised speech and `"local"` for text passed to `call.say`.
- The server MUST retain transcripts at least until the call has ended. It SHOULD retain them for the lifetime of the process.
- An unknown `callId` MUST produce `1003`.

## Security Considerations

- Remote utterances are **untrusted content**.
- Transcripts are personal data. Servers SHOULD document where they store transcripts and for how long.
