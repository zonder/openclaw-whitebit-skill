# ClawHub WhiteBIT Trading Skill

MCP-first WhiteBIT trading workflow for OpenClaw, packaged as a ClawHub skill.

Published skill page:
- `https://clawhub.ai/zonder/whitebit`

## What This Skill Does
- Guides safe trade execution (`buy`/`sell`, market/limit).
- Validates parameters against WhiteBIT docs via MCP.
- Enforces pre-trade checks (intent, balance, risk limits).
- Supports ClawHub lifecycle actions (`search`, `install`, `update`, `publish`, `sync`).

## Prerequisites
- OpenClaw installed.
- `clawhub` CLI installed.
- A WhiteBIT API key pair with the permissions you need for trading.
- A trading MCP server configured in your OpenClaw environment.

## What To Consider Before Installing
1. Confirm credential model before live use.
- Decide where trading credentials live:
- In your trading MCP connector (preferred), or
- As runtime env vars for the skill/session.
- Never upload credentials when publishing to ClawHub.

2. Confirm runtime dependencies are available.
- Ensure `clawhub` CLI is installed and authenticated for install/publish/update flows.
- Ensure trading MCP tools are present before attempting execution.

3. Restrict autonomous execution.
- Start with dry-run/sandbox-capable tools whenever possible.
- Require explicit human confirmation before any live trade.

4. Review publish contents.
- Inspect local files before `clawhub publish` or `clawhub sync`.
- Exclude secrets and local artifacts (`.env`, keys, logs, build outputs).

5. Verify manifest/dependency declarations.
- This skill declares runtime requirements in `SKILL.md` frontmatter:
- `requires.bins`: `clawhub`
- `requires.env`: `WHITEBIT_API_KEY`, `WHITEBIT_API_SECRET`
- `primaryEnv`: `WHITEBIT_API_SECRET`
- If your deployment model differs, update metadata before trusting live execution.

## Install

### Option 1: From ClawHub
```bash
clawhub install whitebit
```

Then start a new OpenClaw session so the skill is loaded.

To pull latest updates later:

```bash
clawhub update whitebit
```

### Option 2: From GitHub
1. Clone this repository.
2. Put this folder in your OpenClaw skills directory as:
`<workspace>/skills/clawhub-whitebit-trading`
3. Start a new OpenClaw session.

## API Keys Setup (Required)

You need to provide your WhiteBIT API credentials to your trading MCP server.
This skill metadata declares `WHITEBIT_API_KEY` and `WHITEBIT_API_SECRET` as required runtime credentials.

Example environment variables:

```bash
export WHITEBIT_API_KEY="your_public_api_key"
export WHITEBIT_API_SECRET="your_secret_api_key"
```

If your MCP server uses config-based env injection, use the same variables in that server config:

```json
{
  "mcpServers": {
    "whitebit-trading": {
      "env": {
        "WHITEBIT_API_KEY": "your_public_api_key",
        "WHITEBIT_API_SECRET": "your_secret_api_key"
      }
    }
  }
}
```

Use the exact variable names expected by your MCP server implementation.

## Quick Verification
1. In OpenClaw, run: `What MCP tools do you have available?`
2. Confirm both:
- Trading MCP tools (place/cancel/status/balance style actions)
- WhiteBIT docs MCP access
3. Run a small or dry-run trade workflow first.

## Security Notes
- Never commit API keys or secrets to Git.
- Use least-privilege API permissions.
- Rotate keys immediately if exposed.
