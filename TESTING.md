# Manual end-to-end tests

Run this with a fresh, real Complete account in each assistant before submitting Complete to that assistant's directory. Only a person running it can mark an assistant verified. Record the result, date, tester and notes for any failure.

Server address: `https://mcp.completelabs.com`. Use a new account created during the test (sign up on Complete's sign-in page), aged 18 or older so every assistant applies; Claude requires 18 or older, the others 13 or older.

## Checklist for each assistant

- [ ] Adding the server with only the URL works (no client ID needed)
- [ ] The OAuth sign-in page loads and sign-up for a new account works
- [ ] The age question appears on first connect
- [ ] Requested scopes are shown and approval completes
- [ ] `list-complete-activities` returns the account's (empty) list
- [ ] `propose-complete-activity` creates a draft and the card renders (or the text fallback is acceptable where cards aren't supported)
- [ ] The Invite button in the card works (ChatGPT and Claude only; elsewhere, inviting from the activity's share page on the Complete website works); the assistant never sends invitations itself
- [ ] Revoking access on the Complete website stops the assistant's access

## Results

| Assistant | How it connects | Result | Date | Tester | Notes |
|---|---|---|---|---|---|
| Claude | Web custom connector | Not run | | | |
| Claude Code | `.mcp.json` | Not run | | | |
| Gemini | gemini.google.com custom app | Not run | | | |
| Gemini CLI | `gemini-extension.json` (read-only tools) | Not run | | | |
| Grok | grok.com custom connector | Not run | | | |
| Muse | Custom connector | Not run | | | |
| ChatGPT | Custom MCP server (regression) | Not run | | | |

Gemini CLI and Codex are limited to the read-only tools, so the draft and card checks do not apply there.

## Age checks (once)

- [ ] A 15-year-old account can connect ChatGPT and create a draft.
- [ ] The same account is refused in Claude, with a message that Claude is for people 18 or older.
