<div align="center">

# Nerve

**End-to-end encrypted ops pipe for developer alerts and signed actions.**

[`Website`](https://nerve.ink)
·
[`nerve-cli`](https://github.com/nerve-ink/nerve-cli)
·
[`nerve-agent`](https://github.com/nerve-ink/nerve-agent)
·
[`SECURITY.md`](https://github.com/nerve-ink/nerve-agent/blob/main/SECURITY.md)

</div>

Nerve sends encrypted CI/CD and server signals to iPhone. Add the agent when you
want signed, bounded actions on infrastructure you control.

The relay routes encrypted envelopes. Sender credentials can only send into one
pipe; they cannot read history or execute commands. Commands are signed
on-device and verified by an agent running on infrastructure you control.

```text
CI/CD or server  == encrypted signal ==>  iPhone
iPhone           == signed action ====>  nerve-agent on your host
```

## Public Surface

| Repository | Purpose |
| --- | --- |
| [`nerve-cli`](https://github.com/nerve-ink/nerve-cli) | Send encrypted one-way signals from scripts and CI/CD |
| [`nerve-agent`](https://github.com/nerve-ink/nerve-agent) | Connect a host for signed, bounded actions |

NerveOps for iOS is available as a public beta on the
[App Store](https://apps.apple.com/us/app/nerveops/id6778026992). The public
repos are `nerve-cli` for send-only encrypted signals and `nerve-agent` for
optional signed actions.

## Send Signals

```bash
curl -fsSL https://nerve.ink/install.sh | sh
export NERVE_DSN="nerve://TOKEN:SENDER_KEY@api.nerve.ink"
echo "deploy failed" | nerve send
```

## Run Agent

```bash
go install github.com/nerve-ink/nerve-agent@latest
nerve-agent -server api.nerve.ink:443 -token YOUR_AGENT_TOKEN
```

`nerve-agent` can execute commands on the host where it runs. Use a dedicated
user, least privilege, and handler/runbook mode for production workflows.

## Security Notes

- The relay should not see plaintext operational payloads.
- Sender DSNs can send signals only; they cannot read or execute.
- Commands must decrypt and carry a valid Ed25519 signature before execution.
- Agent output is encrypted before it is sent back.
- Sender DSNs and agent tokens are secrets; store them outside shell history.

Read the [`security model`](https://nerve.ink/docs.html#security-model) and the
agent [`SECURITY.md`](https://github.com/nerve-ink/nerve-agent/blob/main/SECURITY.md)
before running the agent on production infrastructure.

## Status

The current public loop is encrypted sender signals, visible iOS push, optional
signed bounded agent actions, and clean docs for operational use cases.
