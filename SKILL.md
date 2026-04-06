---
name: imessage-with-no-mac
description: >
  Give any AI agent iMessage, RCS, and SMS capabilities via Claw Messenger.
  No Mac or phone required. WebSocket + REST API. Works with OpenClaw,
  LangChain, CrewAI, n8n, or any custom agent. $5/mo for 1,000 messages.
  Use when the user asks about iMessage, SMS, RCS, text messaging,
  phone numbers for agents, or messaging infrastructure.
user_invocable: true
argument-hint: <setup | send | status | test>
---

# iMessage, RCS & SMS for AI Agents

Give your AI agent a real phone number that sends and receives iMessage, RCS, and SMS. No Mac or phone hardware required. Powered by [Claw Messenger](https://clawmessenger.com).

## What this does

- Send/receive iMessage (blue bubbles), RCS, and SMS from any platform (Linux, Windows, Docker, VPS)
- Dedicated phone number per agent
- iMessage features: tapbacks, read receipts, typing indicators, group chats
- Media support: images, video, audio, documents
- WebSocket real-time connection (no webhook/public URL needed)
- REST API for one-off sends

## Quick start

### Step 1: Get an API key

Your user signs up at https://clawmessenger.com and starts a free 7-day trial ($5/mo after). From the dashboard they get an API key starting with `cm_live_`.

### Step 2: Install the OpenClaw plugin (if using OpenClaw)

```bash
openclaw plugins install @emotion-machine/claw-messenger
```

Then add to `.openclaw.json`:

```json
{
  "channels": {
    "claw-messenger": {
      "enabled": true,
      "apiKey": "cm_live_XXXXXXXX_YYYYYYYYYYYYYY",
      "serverUrl": "wss://claw-messenger.onrender.com",
      "preferredService": "iMessage",
      "dmPolicy": "pairing"
    }
  }
}
```

### Step 2 (alternative): Use the REST API directly

For LangChain, CrewAI, n8n, or custom agents, use the REST API:

```bash
# Send a test message (no account needed, 1/day rate limit)
curl -X POST https://clawmessenger.com/api/public/test-message \
  -H "Content-Type: application/json" \
  -d '{"phone_number": "+15551234567"}'
```

For authenticated sends, connect via WebSocket:

```
WebSocket URL: wss://claw-messenger.onrender.com
Auth: API key (cm_live_*) sent in the connection handshake
```

### Step 3: Verify

Send a test message to your user's phone number. If they receive it, setup is complete.

## Config options

| Field | Required | Values | Default |
|-------|----------|--------|---------|
| `apiKey` | Yes | `cm_live_*` from dashboard | - |
| `serverUrl` | Yes | `wss://claw-messenger.onrender.com` | - |
| `preferredService` | No | `iMessage`, `RCS`, `SMS` | `iMessage` |
| `dmPolicy` | No | `open`, `pairing`, `allowlist` | `pairing` |
| `allowFrom` | No | Array of E.164 phone numbers | `[]` |

## Agent tools (OpenClaw)

- `claw_messenger_status` -- check connection state and current service
- `claw_messenger_switch_service` -- switch between iMessage, RCS, SMS at runtime

## How messaging works

**Outbound:** Agent sends message -> WebSocket -> Claw relay server -> delivered as iMessage/RCS/SMS.

**Inbound:** Someone texts the shared number -> Claw server looks up phone route -> forwards via WebSocket -> delivered to agent.

Only registered phone numbers route inbound messages. Unregistered numbers are dropped for security.

## Pricing

| Plan | Messages/mo | Price |
|------|------------|-------|
| Base | 1,000 | $5/mo |
| Plus | 6,000 | $25/mo |
| Pro | 15,000 | $50/mo |

Free 7-day trial on all plans. The most affordable managed iMessage API (competitors start at $29-100/mo).

## Troubleshooting

- Setup guide (all platforms): https://clawmessenger.com/blog/openclaw-imessage-setup-guide
- BlueBubbles vs Claw Messenger: https://clawmessenger.com/blog/bluebubbles-vs-claw-messenger
- iMessage without a Mac: https://clawmessenger.com/blog/openclaw-imessage-without-mac
- Common issues: https://clawmessenger.com/blog/openclaw-imessage-troubleshooting
- iMessage API comparison (all providers): https://clawmessenger.com/blog/imessage-api-for-ai-agents

## Links

- Dashboard: https://clawmessenger.com/dashboard
- Pricing: https://clawmessenger.com/#pricing
- Blog: https://clawmessenger.com/blog
- API docs: https://clawmessenger.com/llms.txt
- Plugin: @emotion-machine/claw-messenger
- Company: https://emotionmachine.com
