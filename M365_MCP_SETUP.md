# M365 MCP — Calendar & Email Setup Guide

This guide explains how to connect Bob to your Microsoft 365 calendar and email via the
Graph API so Bob can pull your meeting log, attendee lists, and email threads **live** —
instead of you manually exporting and formatting them.

When connected, prompts `02-gather-your-data.md` and `03-build-the-tracker.md` can pull
your last 90 days of calendar data and attendee details automatically, saving hours of
manual formatting.

---

## How it works

Bob reads a Bearer token you paste from Microsoft Graph Explorer. The token is stored in
`m365-mcp/token.txt` and passed as an `Authorization` header on every Graph API call.

Bob calls the Graph API directly using `execute_command` + `curl` — no MCP tool surface
required. The token is read from the file at call time, so Bob can query your calendar or
mailbox on demand without restarting.

**What Bob can pull with this setup:**

| Graph endpoint | What it gives you |
|---|---|
| `me/calendarView` | Every meeting in a date range — subject, start, end, organizer |
| `me/calendarView` + `$select=attendees` | Full attendee list with email addresses per meeting |
| `me/messages` | Inbox and sent mail — subject, sender, recipients, body preview |
| `me/mailFolders/sentItems/messages` | Sent-mail log for the last 90 days |
| `me/events/{id}` | Full detail on a specific calendar event |

**What this replaces:**
- Manual Outlook calendar export → CSV
- Hand-typing attendee names into the meeting log
- Guessing who is IBM vs. partner vs. customer (email domain resolves this automatically)

---

## ⚠️ IBM / corporate tenant constraint

If you are on an IBM or other enterprise Microsoft 365 tenant, you likely **cannot** use
OAuth device-code flow or register your own Entra app. IBM's Entra tenant blocks admin
consent for third-party apps. The approaches that fail and why:

| Approach | Why it fails |
|---|---|
| `npx` MCP packages with device-code auth | IBM blocks admin consent for third-party apps |
| Azure Portal app registration | Requires permissions IBM IT has not granted |
| Personal Microsoft account | Has no access to IBM tenant calendar/mail data |

**The one app IBM has pre-consented:** Microsoft Graph Explorer
(`developer.microsoft.com/graph/graph-explorer`). All setup below uses this.

---

## One-time setup

### Step 1 — Get your first token

1. Go to **https://developer.microsoft.com/graph/graph-explorer**
2. Sign in with your work Microsoft account (e.g. `Firstname.Lastname@ibm.com`)
3. Confirm your name appears in the top-right corner
4. Click **Run query** on the default `GET /me` endpoint — this forces a token to be issued
5. Click the **Access token** tab in the response panel
6. Copy the full `eyJ...` string (it will be very long — copy all of it)

### Step 2 — Store the token in Bob

In Bob (Agent mode), say:

> **"Update my m365 token"** and paste the token

Bob will write it to `m365-mcp/token.txt`. No restart needed.

Alternatively, paste the token directly into the file:

```
m365-mcp/token.txt
```

The file should contain only the raw token string — no quotes, no newline.

### Step 3 — Verify it works

In Bob (Agent mode), say:

> **"What meetings do I have this week?"**

Bob will run:

```bash
TOKEN=$(cat m365-mcp/token.txt)
curl -s -H "Authorization: Bearer $TOKEN" \
  "https://graph.microsoft.com/v1.0/me/calendarView \
  ?startDateTime=YYYY-MM-DDT00:00:00Z \
  &endDateTime=YYYY-MM-DDT23:59:59Z \
  &\$select=subject,start,end,organizer,attendees \
  &\$orderby=start/dateTime&\$top=50"
```

If you see your calendar events in the response, you're connected.

---

## Token refresh (every ~1 hour)

Graph Explorer tokens expire after approximately 1 hour. You will get a `401 Unauthorized`
error when the token has expired.

**To refresh:**

1. Return to **https://developer.microsoft.com/graph/graph-explorer**
2. Confirm you are still signed in (check top-right)
3. Click **Run query** on `/me` to force a new token
4. Click the **Access token** tab
5. Copy the new `eyJ...` string
6. In Bob, say: **"update my m365 token"** and paste the new token

Bob writes it to `token.txt`. Works immediately on next call.

---

## What Bob can do once connected

### Pull your meeting log automatically

```
Pull my calendar from [START DATE] to [END DATE] and format it as a meeting log
for my partner tracker. For each meeting include: date, subject, organizer,
attendees (broken out as IBM / [PARTNER NAME] / External based on email domain),
and a one-line summary. Skip personal appointments.
```

### Identify IBM vs. partner vs. customer attendees

Bob resolves attendee org from email domain automatically:

| Domain pattern | Classification |
|---|---|
| `@ibm.com`, `@*.ibm.com`, `@us.ibm.com` | IBM |
| `@[partnerdomain].com` | Partner (Technologent, etc.) |
| `@arrow.com`, `@ingrammicro.com` | Distributor |
| Everything else | External / Customer |

### Pull sent mail for the email log

```
Pull my sent emails from [START DATE] to [END DATE]. For each thread, give me:
date, subject, recipients, and a one-line summary of what was delivered or requested.
Filter to emails related to [PARTNER NAME] or [IBM PRODUCTS].
```

### Query attendees for a specific meeting

```
Get the full attendee list for my "[MEETING SUBJECT]" meeting on [DATE].
Classify each attendee as IBM, [PARTNER NAME], or External based on their email domain.
```

---

## Bob config entry

The `m365-mcp` server is registered as a local Node.js server in `.bob/mcp.json`.
It connects at startup and stays connected regardless of token state — the token is only
read when a tool is called.

```json
"m365-mcp": {
  "command": "node",
  "args": ["./m365-mcp/build/index.js"],
  "disabled": false
}
```

The server source lives in `m365-mcp/` in this workspace. It exposes these tools
(available when the MCP panel shows the server as Connected):

| Tool | Description |
|---|---|
| `list_calendar_events` | List upcoming calendar events |
| `get_calendar_event` | Get full details of a specific event |
| `create_calendar_event` | Create a new calendar event |
| `list_emails` | List emails from inbox or any folder |
| `read_email` | Read full content of an email |
| `send_email` | Send an email |
| `search_emails` | Search across mailbox by keyword |
| `reply_to_email` | Reply or reply-all to an email |

> **Note:** Even if the MCP tools are not surfaced in Bob's tool panel, Bob can call the
> Graph API directly via `curl` using the token in `token.txt`. Both approaches use the
> same token and the same Graph endpoints — the curl approach requires no MCP tool surface.

---

## Permanent fix (requires IT)

To eliminate manual token refreshes entirely, ask your IT team to:

1. Register an Entra app in your Microsoft tenant
2. Grant delegated permissions: `Calendars.ReadWrite`, `Mail.ReadWrite`, `Chat.Read`
3. Grant admin consent for the app

Once done, the MCP server can handle OAuth and token refresh automatically.
