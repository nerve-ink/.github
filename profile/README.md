<div align="center">

# Nerve

**Zero-knowledge ops pipes for encrypted signals and signed recovery commands.**

[`nerve-agent`](https://github.com/nerve-ink/nerve-agent)
·
[`SECURITY.md`](https://github.com/nerve-ink/nerve-agent/blob/main/SECURITY.md)

</div>

Nerve is a private-by-default infrastructure project for developers who want an
encrypted control loop between their phone and the servers they operate.

The relay only handles ciphertext. Commands are signed on-device and verified by
an agent running on infrastructure you control.

```text
iOS device  <== encrypted signals / signed commands ==>  nerve-agent
                 via zero-knowledge relay
```

## What We Are Building

- End-to-end encrypted signals from servers to iOS.
- Signed commands from iOS back to `nerve-agent`.
- A small public agent surface that can be installed with Go.
- Private app, backend, and integration code while V1 is being hardened.

## Repository Surface

| Repository | Visibility | Purpose |
| --- | --- | --- |
| [`nerve-agent`](https://github.com/nerve-ink/nerve-agent) | Public | Installable server-side daemon |
| iOS app | Private | Native encrypted pipe client |
| backend relay | Private | Auth, sync, WebSocket routing, encrypted storage |
| CLI | Private | Future pipe utility |

## Public Repository

[`nerve-agent`](https://github.com/nerve-ink/nerve-agent) is the public
server-side daemon for Nerve.

```bash
go install github.com/nerve-ink/nerve-agent@latest
nerve-agent -server api.nerve.ink:443 -token YOUR_AGENT_TOKEN
```

## Security Model

- The relay should not see plaintext operational payloads.
- Commands must decrypt before execution.
- Commands must include a valid Ed25519 signature.
- Agent output is encrypted before it is sent back.
- Agent tokens are secrets and should be stored outside shell history.

Read the agent
[`SECURITY.md`](https://github.com/nerve-ink/nerve-agent/blob/main/SECURITY.md)
before running it on production infrastructure.

## Status

Nerve is early. The current focus is the V1 loop:

1. Create an encrypted pipe in the iOS app.
2. Connect `nerve-agent`.
3. Receive encrypted signals.
4. Send signed commands.
5. Receive encrypted command output.

The iOS app, backend relay, and CLI are private while that loop is stabilized.
