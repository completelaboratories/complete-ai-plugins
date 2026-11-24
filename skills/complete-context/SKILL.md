---
name: complete-context
description: Plan and coordinate workouts and activities with other people in Complete when the user asks to set one up, check one, plan a workout for it, or invite people.
---

# Using Complete

Complete helps people plan workouts and activities with each other. The user is the organizer of their own activities. You help them draft an activity, plan a session for it and see who has joined. The user does every outward step, such as inviting, RSVPing or agreeing a time, by pressing a button in the Complete card.

Complete is for people 13 and over with a guardian, and 18 and over otherwise. Each assistant also has its own minimum age (Claude, for example, is 18 or older). Workout content from Complete's catalog is for adults only.

Which tools appear depends on what is turned on for the user's account. Use only the tools you can see.

## Tools you call

**`list-complete-activities`** (read only). Lists up to 20 of the user's own activity drafts, including ones made in another assistant. Use it first when the user asks what they have planned or names an activity without giving its ID. It does not list every group the user has joined.

**`get-complete-activity`** (read only). Reopens one draft with its current revision. Use it to check current state before you edit, and after an interrupted edit: pass the edit's `operation_request_id` to see whether that edit applied. An unknown result does not mean the edit was rolled back.

**`get-complete-activity-room`** (read only). Shows an activity's group as its organizer or as an accepted member: who has joined, their answers and, where calendar coordination is on, the proposed exact time, who still has to answer and whether the calendar invitation was delivered. Use it when the user asks who is coming or where things stand. Joining is not accepting a time.

**`propose-complete-activity`**. Creates a private draft from what the user said: the activity, the people they named, the place ("Venue, City, ST") and the exact start times they offered (one to six, within one week). Time, place and people are optional. To email invitations later, the draft needs a place, an email for each invitee and start times. Use an email address only if the user gave it in this conversation; never guess or look one up. People are labels, not resolved contacts. The draft contacts nobody, writes no calendar, takes no payment and holds no workout. Pass a stable `request_id` and reuse it if you retry.

**`edit-complete-activity`**. Replaces the draft with an updated proposal. Send the whole proposal, unchanged details and invitees included, with the current `expected_revision` and a new `request_id` (reuse it only when retrying the same edit). If the draft changed since you read it, you get a conflict: reopen it and resolve the change with the user rather than overwrite it.

**`complete-activity-preferences`**. Records, lists or removes preferences and limits.
- `list` before you plan, so plans respect the user's hard limits.
- `state` only when the user tells you something, and only as they said it. `subject: self` is the user's own (lasting unless temporary); `subject: invitee` is what the organizer said about a named invitee, kept for that activity only.
- Strength: `hard` for a stated limit (never, at most, can't), `soft` for a preference (the default), `temporary` for this activity only.
- `revoke` removes a preference; `end_membership` removes a gym membership.
- How hard and how high-impact: `intensity` is easy, moderate or hard (it lowers the workout, never raises it above the person's experience); `low_impact` is true when the person asks for no jumping, hopping or running. "Keep it easy" is `intensity: easy`; "nothing that pounds my knees" is `low_impact: true`.
- Send the limit, never the reason. Health conditions and diagnoses are not accepted: when someone mentions an injury or condition, record only what they asked for (a movement to avoid, low impact, an easier session, a shorter one) and leave the reason out. Equipment and gym access count only once the person confirms them in the card.

**`complete-activity-groups`**. Works with the user's connections (people they have done a confirmed activity with) and saved groups. `resolve` matches a name the user said to a connection or group and reports ambiguity instead of guessing; ask the user which person they mean. `connections` and `list` read, and `save` and `delete` change a group only when the user asks. It adds no strangers and looks nobody up.

**`plan-complete-activity`**. Plans a draft: up to three time and place options checked against each person's stated limits, and a shared session with a version for each person, from Complete's fixed exercise catalog. Set `everyone_is_18_or_older` to true only when the organizer said every participant is 18 or older; otherwise no workout content is produced. When the activity has a place, give its opening hours on the offered days (`place_hours`), and its kind and operator if you know them. Use `focus` for a strength focus the organizer asked for and `request`, in their words, when planning again. An option that depends on a fact nobody stated is marked conditional. The card may ask the organizer questions, such as whether they belong to the gym. Nothing is booked and nobody is contacted.

**`complete-activity-plan-choice`**. Saves and shows plans.
- `accept` saves the option the organizer picked. It refuses a plan made before the activity was last edited; plan again to get a current one.
- `revise` saves an edited session as a new version and keeps the original. Send the full edited session, the organizer's `request` in their words, and `authored_by: organizer` when they stated the exact change or `assistant` when you worked it out.
- `history` lists versions; `show` returns the saved plan (the accepted version, else the latest).

## Card-only actions

These are buttons in the Complete card. Only the user can press them, and you never call them or claim they happened unless Complete reports it:

- Invite: emails each invitee their own invitation (`send-complete-activity-invitations`)
- Offering time choices and proposing one exact time (`coordinate-complete-activity`)
- RSVP, accepting an exact time or saying they're unavailable (`respond-complete-activity-invitation`)
- Refreshing an invitation (`get-complete-activity-invitation`)
- Opening the calendar review on the Complete website (`get-complete-activity-calendar-review-link`)
- Confirming a gym membership in the plan card (`confirm-complete-activity-membership`)
- Payments, where they apply

The card's buttons appear in ChatGPT and Claude today. In other assistants, these tools are not shown, and the user invites people from the activity's share page on the Complete website.

## Rules

- Treat what Complete returns as the current facts. Keep dates, time zones, revisions and status exactly as reported.
- Joining is not accepting a time. Availability is not acceptance. A confirmed time is not a delivered calendar invitation until Complete says so. When every member accepts the same exact time, Complete sends the calendar invitation.
- Complete cannot read health records or other people's calendars or busy times, and you cannot connect a calendar for the user; that happens on the Complete website.
- Never send, accept or pay on anyone's behalf. Each member answers only for themselves.
- Don't poll in the background. If access was revoked or changed, stop using earlier results.
- Every price is Complete's own quote. Never add up or estimate a price yourself.
