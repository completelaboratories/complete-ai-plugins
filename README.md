# Complete for AI assistants

Complete helps people plan workouts and activities with each other. Its mission
is to make pursuing ambitions a shared experience. Chase together.

This repository holds the public connection files for Complete's remote MCP
server, so an AI assistant can work with your Complete activities on your
behalf. It contains no backend code and no credentials.

## Status (November 2026)

| | |
|---|---|
| Live MCP server | `https://stg-app.completelabs.com/mcp/complete` |
| Access | Invite-only. Only Complete accounts that Complete has enabled can sign in; it is not open for sign-up yet. |
| Verified client | ChatGPT, as a custom MCP server |
| Other clients | Configs are included but not yet tested against the live server |
| ChatGPT plugin directory | Not submitted. Complete is not a listed ChatGPT plugin. |

`stg-app.completelabs.com` is the address of Complete's live service. If it
changes, this README and the config files will change with it.

## What it does

With your permission, the assistant can:

- draft a private activity (what, where, when, who) and edit it
- plan a workout for the activity from Complete's fixed workout catalog
- show an activity and its group room: who has joined, and their answers
- help you invite people by email and agree on a time

You stay in control of every outward step. Invitations go out only when you
press **Invite** in the Complete card, and RSVPs, time offers and accepting an
exact time are actions you take in the card. When every member accepts the
same time, Complete sends the calendar invitation.

The assistant cannot read health records, other people's calendars or busy
times, and cannot connect your calendar for you. Calendar connection stays on
the Complete website. You can withdraw an assistant's access to your
activities at any time on the Complete website.

## Connect from ChatGPT

You need a Complete account that Complete has enabled for ChatGPT.

1. In ChatGPT, go to **Plugins**, select the **+** button, then
   **Add custom MCP server**. Your ChatGPT plan and workspace settings decide
   whether you can add custom MCP servers.
2. Enter the name **Complete** and a short description.
3. Under **Connection**, enter
   `https://stg-app.completelabs.com/mcp/complete`.
4. Set authentication to **OAuth** and enter the OAuth client details Complete
   gave you when it enabled your account. ChatGPT can't fill these in by
   itself yet, because Complete's sign-in service doesn't publish a discovery
   document.
5. Review the risk warning, select **I understand and want to continue**, then
   **Create as a plugin**.
6. Sign in with your Complete account and approve the requested permissions.
7. Start a new chat, type **@**, choose Complete, and ask something like
   "Plan a Saturday morning run with two friends."

These steps follow OpenAI's
[connect and test guide](https://developers.openai.com/apps-sdk/deploy/connect-chatgpt)
as of November 2026. If ChatGPT's menus look different, that guide has the
current steps.

Complete uses OAuth only. Never set authentication to **No authentication**,
and never paste a token or password into a config file.

## Other clients (untested)

These files point at the same server. They are provided for people who want
to try them; none has completed sign-in against the live server yet.

| Client | File |
|---|---|
| Claude (remote connector / Claude Code) | `.mcp.json`, `clients/claude.mcp.json` |
| Codex | `clients/codex.toml` (disabled by default) |
| Gemini CLI | `gemini-extension.json`, `GEMINI.md` |
| Antigravity | `adapters/antigravity/` (disabled by default) |
| Agent Plugins manifest | `plugin.json`, `mcp.json` |

## How sign-in works

The server publishes its OAuth protected-resource metadata at
`https://stg-app.completelabs.com/.well-known/oauth-protected-resource/mcp/complete`.
It names the authorization server and the scopes. An unauthenticated request
returns `401` with a `WWW-Authenticate` header that points to that document.
The authorization server doesn't publish OAuth discovery metadata yet, so
clients need the endpoints and client details entered by hand.

## Support

support@completelabs.com · https://completelabs.com/support/

Privacy: https://completelabs.com/privacy/

## License

The configuration files and instructions listed in
[license-scope.json](license-scope.json) are licensed under the
[MIT License](LICENSE), copyright 2026 Complete Laboratories, Inc. The PNG
brand assets are excluded and reserved under
[assets/NOTICE.md](assets/NOTICE.md). The license does not cover the hosted
service, its data, the backend, or Complete's names and trademarks, and it
grants no access to the service. Files added later need an explicit scope
decision.
