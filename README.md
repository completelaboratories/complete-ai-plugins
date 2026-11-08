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
| Access | Invite-only. You need a Complete account that is on the invite list. |
| Verified client | ChatGPT, as a custom connector in developer mode |
| Other clients | Configs are included but not yet tested against the live server |
| ChatGPT app directory | Not submitted. Complete is not a listed ChatGPT app. |

The server is Complete's live service. The `stg-app` hostname is historical;
a dedicated `mcp.completelabs.com` address is planned, and this README will
change when it exists.

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
the Complete website.

## Connect from ChatGPT

1. Ask Complete (support@completelabs.com) to add your account to the invite
   list.
2. In ChatGPT, open **Settings → Apps & Connectors → Advanced settings** and
   turn on **Developer mode**.
3. Under **Apps & Connectors**, choose **Create** and enter:
   - **Name:** Complete
   - **MCP server URL:** `https://stg-app.completelabs.com/mcp/complete`
   - **Authentication:** OAuth
4. Complete the OAuth settings you were given with your invitation, then
   sign in with your Complete account and approve the requested permissions.
5. Start a chat, enable Complete for that chat, and ask something like
   "Plan a Saturday morning run with two friends."

ChatGPT menu names change from time to time; if a step looks different, the
setting is under developer mode for custom connectors. Developer mode is
available on the ChatGPT plans that OpenAI allows it on.

Complete uses OAuth only. Never set the connector to **No authentication**,
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
Clients that follow the MCP authorization spec read the authorization server
and scopes from there. An unauthenticated request returns `401` with a
`WWW-Authenticate` header that points to that document.

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
