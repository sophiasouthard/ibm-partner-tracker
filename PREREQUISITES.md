# Prerequisites

Everything you need in place before running the prompts in this repo.

---

## 1. Bob (IBM watsonx Code Assistant)

You need Bob installed and running. This entire workflow is Bob-native — no terminal, no Python,
no other tools required.

**Check:** Open Bob. If you can type a prompt and get a response, you're good.

---

## 2. IBM VAR Channel Strategist Mode

This repo includes a custom Bob mode pre-loaded with IBM VAR/PTS channel knowledge. Install it
by copying the `.bob/custom_modes.yaml` file in this repo into your workspace's `.bob/` folder.

If you already have a `.bob/custom_modes.yaml`, append the `ibm-var-channel` entry — do not
overwrite your existing modes.

**After installing:** The mode appears in the Bob mode picker immediately (hot-reload, no restart).
Select **IBM VAR Channel Strategist** before running any of the prompts in this repo.

**What this mode knows:**
- IBM Partner Plus tier structure (Silver / Gold / Platinum)
- VAR vs VAD vs direct — how the two-tier model works
- IBM Software Value Incentive (SVI) margins and how partners earn them
- Deal registration (MySA) — what it is and why it matters
- What "self-sufficient reseller" actually means in practice
- How to distinguish partner effort from partner buy-in
- How to frame the executive conversation when a partner isn't following through

---

## 3. Tavily MCP

Tavily gives Bob live web search. You need this for Step 1 (researching your partner).

**Check:** In Bob, ask: *"Search Tavily for [Your Partner Name] IBM Business Partner."*
If you get results, Tavily is connected.

**If it's not connected:** Ask your Bob admin to add Tavily as an MCP server, or follow the
configure-mcp skill in Bob.

---

## 4. IBM Data & AI Docs MCP (recommended)

Gives Bob access to current IBM product documentation. Useful when building the tracker's
product positioning and pipeline sections.

**Check:** In Bob, ask: *"Search IBM docs for watsonx Orchestrate."*
If you get documentation results, the MCP is connected.

---

## 5. Your Opportunity Data

Export your active pipeline from Salesforce (or wherever your opportunities live).

**Minimum fields needed:**
- Opportunity name
- Account name
- Product / product family
- Stage (Propose / Design / Qualify / Engage or your equivalent)
- Total value ($)
- Expected close date
- Opportunity owner
- Any notes on deal status

**Format:** CSV, Excel, or paste directly into Bob as a table. Bob can handle any of these.

**Tip:** Include stalled and at-risk deals — not just active ones. The tracker's diagnosis depends
on seeing the full picture, not just the healthy pipeline.

---

## 6. Your Meeting Log (last 90 days)

A log of every significant meeting you've had related to this partner. Does not need to be
exhaustive — focus on meetings where something happened: a demo, an enablement session,
a customer touch, a pipeline call, an event.

**Minimum fields per meeting:**
- Date
- Meeting name / topic
- Attendees (names or roles)
- Type (Virtual / In Person)
- Topic category (Enablement / Pipeline / Bob / Confluent / Event / Relationship / etc.)
- Brief outcome or note

**How to gather this:** Export from your calendar (Google Calendar → CSV, Outlook → Excel),
or simply open your calendar for the last 90 days and paste the meeting titles + dates into Bob.
Bob will help you structure them.

---

## 7. Your Email Log (key threads, last 90 days)

You do not need to export every email. Focus on threads where you:
- Sent a research report or architecture assessment
- Coordinated a demo or event
- Introduced an IBM specialist to a partner seller
- Addressed a deal registration or quoting issue
- Received an inbound inquiry from a customer via the partner

**Format:** A simple list of date + subject + who it was with + one-sentence summary is enough.
Bob will structure it into the tracker format.

---

## 8. Your Quota

Your FY target number from your manager. This populates the Dashboard's quota progress bar.

If you have multiple quota components (e.g. software vs. services, or different product families),
note them separately.

---

## 9. Partner Usage Data (optional but high-value)

If your partner has been provisioned with any IBM tools (Bob builder seats, watsonx Orchestrate
tenants, watsonx.ai sandboxes), try to pull:

- **Builder/user list** — who was provisioned
- **Active users** — who has actually logged in
- **Usage metrics** — Bobcoins spent, lines of code generated, agents built, etc.

This data is what turns "we ran enablement sessions" into a concrete diagnosis of whether the
partner's engineers are actually using the tools — or whether you are.

**Where to find it:** Ask your IBM CSM or the tool's admin console. For Bob specifically,
the usage report is downloadable from the Bob admin dashboard.

---

## 10. Partner Contact List

A list of your key contacts at the partner. Cards in the Contacts tab are built from this.

**Minimum fields per contact:**
- Name
- Title / role
- Region or office
- Product focus (what IBM products are they involved with)
- Engagement level (High / Medium / Low)
- Email
- Brief note (what you know about them, what they've done)

---

## Quick Checklist

```
[ ] Bob installed and working
[ ] IBM VAR Channel Strategist mode installed (.bob/custom_modes.yaml)
[ ] Tavily MCP connected
[ ] IBM Data & AI Docs MCP connected (recommended)
[ ] Opportunity export ready (CSV or Excel)
[ ] Meeting log ready (last 90 days)
[ ] Email log ready (key threads, last 90 days)
[ ] Quota number confirmed
[ ] Partner usage data pulled (optional)
[ ] Contact list ready
```

Once all boxes are checked, go to [`prompts/01-research-your-partner.md`](../prompts/01-research-your-partner.md).
