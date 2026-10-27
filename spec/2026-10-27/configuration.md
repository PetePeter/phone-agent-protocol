# Configuration (Non-normative)

**Protocol Revision:** `2026-10-27`

This page is non-normative: PhAP does not define a configuration format. What it does require (see [Security](security.md)) is that the settings below be supplied out of band, at startup. The example uses TOML.

```toml
[device]
# implementation-specific; e.g. a Bluetooth HCI transport
transport = "usb:10D7:B012+sco=2"

[policy]
allowlist      = ["+447700900123", "+61491570157"]
fallback       = "ring"     # ring | reject   (applied after ringTimeout)
ring_timeout_s = 15
outbound       = "allowlist" # allowlist | all   (all: the controller decides; see Security §1)

[phap]
bind               = "127.0.0.1:7781"
token              = "use-a-long-random-value"
heartbeat_s        = 10
event_buffer       = 1000

[voice]
enabled   = true
tts_model = "models/en_GB-alan-medium.onnx"
stt_model = "small.en"

[mcp]
enabled = true              # expose the MCP projection over stdio
```

| Setting | Spec reference |
|---|---|
| `policy.allowlist`, `policy.outbound` | [Security §1](security.md#1-server-side-policy-enforcement) |
| `policy.fallback`, `ring_timeout_s` | [Calls: `call.ringing`](server/calls.md#callringing) |
| `phap.token` | [Authorization](basic/authorization.md) |
| `phap.heartbeat_s` | [Heartbeat](basic/utilities/heartbeat.md) |
| `phap.event_buffer` | [Resumption](basic/utilities/resumption.md) |
