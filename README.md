# Complete for AI assistants

Complete helps people plan workouts and activities with each other. Its mission
is to make pursuing ambitions a shared experience. Chase together.

This repository holds the public connection files for Complete's remote MCP
server, so an AI assistant can work with your Complete activities on your
behalf. It contains no backend code and no credentials.

## Status (November 2026)

| | |
|---|---|
| Live MCP server | `https://mcp.completelabs.com` |
| Access | Invite-only. Only Complete accounts that Complete has enabled can sign in; it is not open for sign-up yet. |
| Verified client | ChatGPT, as a custom MCP server |
| Other clients | Configs are included but not yet tested against the live server |
| ChatGPT plugin directory | Not submitted. Complete is not a listed ChatGPT plugin. |

The server lives at the root of `mcp.completelabs.com`, with no path.

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

1. In ChatGPT, go to **Plugins**, select **Add** (top right), then
   **Add custom MCP server**. Your ChatGPT plan and workspace settings decide
   whether you can add custom MCP servers.
2. Enter the name **Complete** and a short description.
3. Under **Connection**, enter `https://mcp.completelabs.com`.
4. Set authentication to **OAuth**. Leave the client ID and endpoints empty:
   ChatGPT finds them from the server address.
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

## Other clients (not yet supported)

These files point at the same server, but only ChatGPT can sign in today:
Complete's sign-in accepts ChatGPT's client and no other yet. The files are
here so these clients work once Complete supports them.

| Client | File |
|---|---|
| Claude (remote connector / Claude Code) | `.mcp.json`, `clients/claude.mcp.json` |
| Codex | `clients/codex.toml` (disabled by default) |
| Gemini CLI | `gemini-extension.json`, `GEMINI.md` |
| Antigravity | `adapters/antigravity/` (disabled by default) |
| Agent Plugins manifest | `plugin.json`, `mcp.json` |

## How sign-in works

ChatGPT needs only the server address; it discovers the rest.

- Protected-resource metadata:
  `https://mcp.completelabs.com/.well-known/oauth-protected-resource`
- Authorization server metadata:
  `https://mcp.completelabs.com/.well-known/oauth-authorization-server`
- ChatGPT identifies itself with its client ID metadata document (CIMD), so
  there is no client ID to register or enter.
- Scopes: `https://mcp.completelabs.com/activity.read`, `activity.propose`,
  `activity.respond` and `activity.coordinate` under the same prefix.

An unauthenticated request returns `401` with a `WWW-Authenticate` header
that points to the protected-resource metadata.

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
