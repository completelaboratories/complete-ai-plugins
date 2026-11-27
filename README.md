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
| Access | Complete is for people 13 and over with a guardian, 18 and over otherwise. Each assistant also has its own minimum age (Claude: 18 or older). New users create a Complete account on the sign-in page; Complete asks your age the first time you connect. |
| Verified end to end | ChatGPT, as a custom MCP server. Last verified per assistant: see [TESTING.md](TESTING.md). |
| Sign-in open to | Any assistant that supports remote MCP servers with OAuth: Claude, Gemini, Grok, Muse and others can register and reach Complete's sign-in. Not yet tested end to end with a real account. |
| Assistant directories | Not submitted. Complete is not listed in any assistant's plugin or connector directory yet. |

The server lives at the root of `mcp.completelabs.com`, with no path.

## What it does

With your permission, the assistant can:

- draft a private activity (what, where, when, who) and edit it
- plan a workout for the activity from Complete's fixed workout catalog
  (catalog workouts are for adults 18 and over)
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

## Tools

The full guide for assistants, with when to use each tool, is
[skills/complete-context/SKILL.md](skills/complete-context/SKILL.md). Which
tools appear depends on what is turned on for your account.

The assistant can call these:

| Tool | What it does |
|---|---|
| `list-complete-activities` | Lists up to 20 of your own activity drafts (read only) |
| `get-complete-activity` | Reopens one draft and its current revision; checks whether an interrupted edit applied (read only) |
| `get-complete-activity-room` | Shows an activity's group: who has joined, their answers and where the time stands (read only) |
| `propose-complete-activity` | Creates a private draft: activity, place, offered start times and the people you named |
| `edit-complete-activity` | Replaces the draft with an updated proposal at its current revision |
| `complete-activity-preferences` | Records and lists the preferences and limits you state, and your gym memberships; health conditions are not accepted |
| `remove-complete-activity-preference` | Removes a preference or ends a gym membership when you ask (marked destructive) |
| `complete-activity-groups` | Matches a name you say to someone you have done an activity with or to a saved group (read only) |
| `save-complete-activity-group` | Saves a group of people you have done an activity with; saving over a group replaces its members (marked destructive) |
| `delete-complete-activity-group` | Deletes a saved group when you ask (marked destructive) |
| `plan-complete-activity` | Plans up to three time and place options and a shared workout from Complete's catalog (adults only) |
| `complete-activity-plan-choice` | Saves the plan you pick or an edit you ask for as a new version, and shows saved versions |

These six are app-only card actions: buttons in the Complete card. They are
hidden from the model, only you can press them, and the assistant never calls
them:

| Tool | Button |
|---|---|
| `send-complete-activity-invitations` | Invite: emails each invitee their own invitation |
| `coordinate-complete-activity` | Offer time choices or propose one exact time |
| `respond-complete-activity-invitation` | RSVP, accept an exact time, or say you're unavailable |
| `get-complete-activity-invitation` | Refresh an invitation |
| `get-complete-activity-calendar-review-link` | Open the calendar review on the Complete website |
| `confirm-complete-activity-membership` | Confirm whether you belong to the activity's gym |

Payments, where they apply, are also yours to make in Complete, never the
assistant's.

## Connect from ChatGPT

You need a Complete account, or create one when you sign in. Complete is for people 13 and over with a guardian, 18 and over otherwise.

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

## Connect from other assistants

You need a Complete account, or create one when you sign in. Each assistant adds
Complete as a custom remote MCP server with OAuth: enter
`https://mcp.completelabs.com`, leave any client ID or secret empty, then sign
in with your Complete account and approve the requested permissions. Menu
names change often; each assistant's own help has the current steps.

The Complete card's buttons (Invite, RSVP, time offers) appear only in ChatGPT
and Claude today. In other assistants, send invitations from the activity's
share page on the Complete website.

| Assistant | Where to add it | Notes |
|---|---|---|
| Claude | Settings → Connectors → Add custom connector | 18 or older. Claude Code: `.mcp.json` or `clients/claude.mcp.json` |
| Gemini | gemini.google.com → Connected apps → add a custom app ([Google's help](https://support.google.com/gemini/answer/17209137)) | Google limits this to personal accounts in the US, 18 or over |
| Grok | grok.com → Connectors → New connector → Custom | xAI limits custom connectors to some plans |
| Muse (Meta AI) | Add a custom connector in Muse | |
| Codex | `clients/codex.toml` (disabled by default) | Limited to the read-only tools |
| Gemini CLI | `gemini-extension.json`, `GEMINI.md` | Limited to the read-only tools |
| Antigravity | `adapters/antigravity/` (disabled by default) | |
| Agent Plugins manifest | `plugin.json`, `mcp.json` | |

These connections reach Complete's sign-in today but have not yet been tested
end to end with a real account. If one fails, write to
support@completelabs.com.

## How sign-in works

An assistant needs only the server address; it discovers the rest.

- Protected-resource metadata:
  `https://mcp.completelabs.com/.well-known/oauth-protected-resource`
- Authorization server metadata:
  `https://mcp.completelabs.com/.well-known/oauth-authorization-server`
- Assistants identify themselves with a client ID metadata document (CIMD),
  as ChatGPT does, or register automatically (dynamic client registration at
  `https://mcp.completelabs.com/oauth/register`), so there is no client ID to
  enter. PKCE (S256) is required.
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
