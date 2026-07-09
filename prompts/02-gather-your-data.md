# Prompt 02 — Gather and Format Your Data

**Mode:** IBM VAR Channel Strategist (or default Agent mode)  
**MCP tools needed:** M365 MCP (recommended) or none  
**When to run:** After Step 1. You should have your opportunity export ready, and either
M365 MCP connected for live calendar/email pull, or your meeting/email logs manually prepared.
See [`PREREQUISITES.md`](../PREREQUISITES.md) for what to prepare.

---

## Overview

This step has four sub-prompts — one for each data source. Run them in order.
Each produces a structured table you'll feed into the tracker-building prompt in Step 3.

**If you have M365 MCP connected:** Use the live-pull prompts in **2B-Live** and **2C-Live**
instead of 2B and 2C. Bob will pull your calendar and sent mail directly from Microsoft 365
and classify attendees by org (IBM / partner / customer) automatically.

**If you don't have M365 MCP:** Use the manual formatting prompts 2B and 2C as written.
See [`M365_MCP_SETUP.md`](../M365_MCP_SETUP.md) to get connected — it takes about 5 minutes
and saves hours of manual formatting.

---

## 2A — Format Your Opportunity Pipeline

```
I need to format my pipeline data for a client analytics tracker. Here is my raw opportunity data:

[PASTE YOUR CSV/EXCEL DATA OR TYPE IT OUT AS A TABLE]

For each opportunity, I need these fields:
- Opportunity ID (from Salesforce, or make one up if you don't have it — e.g. OPP-001)
- Opportunity name
- Account name
- Product (map to one of: IBM Bob, Confluent, watsonx Orchestrate, watsonx.data, watsonx.ai,
  Security, Renewal/Modernization, or Other)
- Status: Active / At Risk / Stalled / Active No Pay / Need Check
  - Active = progressing with committed next steps
  - At Risk = stalled or missing key milestones, competitor present, or seller disengaged
  - Stalled = no activity, no next steps
  - Active No Pay = real deal but you're not on the comp plan for this account
  - Need Check = you haven't verified this recently
- Dollar value (total $, not just your filtered share)
- Stage (Propose / Design / Qualify / Engage)
- Expected close date
- Opportunity owner (partner AE or IBM rep name)
- One-line note on current status

Also tell me:
- Which deals came from partner-sourced activity vs. IBM-sourced vs. event vs. inbound?
- Which deals have an active partner AE engaged? Which are being driven by IBM only?
- Are there any deals where you're not on the comp plan?

Format the output as a markdown table with all the fields above.
```

---

## 2B-Live — Pull Your Meeting Log from M365 Calendar (requires M365 MCP)

*Use this instead of 2B if you have M365 MCP connected.*

```
Pull my Outlook calendar from [START DATE e.g. 2026-04-01] to [END DATE e.g. 2026-07-09]
using the Graph API and format it as a meeting log for my partner tracker.

My partner is: [PARTNER NAME]
Their email domain is: [e.g. technologent.com]
My IBM colleagues are at: ibm.com and all *.ibm.com subdomains
My distributor is: [e.g. Arrow — arrow.com / Ingram Micro — ingrammicro.com]

For each meeting that is relevant to this partnership (skip personal appointments, 
dentist, dry cleaners, etc.), give me:
- Date (YYYY-MM-DD)
- Meeting name
- Topic category: Bob / Confluent / Pipeline / Enablement / Event / Relationship /
  Marketing / Modernization / Security / watsonx / Orchestrate
- Contacts — broken out as:
  - IBM: [names at @ibm.com or @*.ibm.com]
  - [PARTNER NAME]: [names at @partnerdomain.com]
  - Distributor: [names at @arrow.com / @ingrammicro.com etc.]
  - External/Customer: [everyone else]
- Type: Virtual or In Person (infer from whether it has a Teams link or physical location)
- One-sentence outcome or note

After formatting, tell me:
- How many meetings were customer-facing (included a named end-user) vs. partner-only?
- How many were enablement sessions? Did attendance grow or decline?
- Are there any meetings where a [PARTNER NAME] engineer or AE led something independently?
- Are there meetings with no traceable commercial outcome?

Format as a markdown table.
```

---

## 2B — Format Your Meeting Log (manual — use if M365 MCP is not connected)

```
I need to format my meeting log for a partner tracker. Here is my raw meeting data:

[PASTE YOUR CALENDAR EXPORT OR TYPE OUT YOUR MEETINGS — date, title, attendees, outcome]

For each meeting, I need:
- Date (YYYY-MM-DD)
- Meeting name
- Topic category: Bob / Confluent / Pipeline / Enablement / Event / Relationship / Marketing /
  Modernization / Security / watsonx
- Key attendees (first and last name, or role if name unknown)
- Type: Virtual or In Person
- One-sentence outcome or note

After formatting, tell me:
- How many meetings were customer-facing (included a named end-user customer) vs. partner-only?
- How many were enablement sessions? Did attendance grow or decline over time?
- Are there any meetings where a partner engineer or AE led something independently
  (without you running it)?
- Are there any meetings with no traceable commercial outcome — no opp created, no follow-up
  logged, no action item?

Format the output as a markdown table.
```

---

## 2C-Live — Pull Your Email Log from M365 Sent Mail (requires M365 MCP)

*Use this instead of 2C if you have M365 MCP connected.*

```
Pull my sent emails from [START DATE e.g. 2026-04-01] to [END DATE e.g. 2026-07-09]
using the Graph API (me/mailFolders/sentItems/messages).

My partner is: [PARTNER NAME] — domain [e.g. technologent.com]
Focus on emails related to this partner or to IBM products (Bob, Confluent, watsonx,
Orchestrate, Guardium, Db2, etc.).

For each relevant sent email thread, give me:
- Date (YYYY-MM-DD)
- Subject / thread name
- Topic: bob / confluent / watsonx / security / renewal / research / event / marketing / pipeline
- Key recipients — classified as IBM / [PARTNER NAME] / Customer / Other
- Direction: Outbound (I sent) / Inbound (I received) / Thread (ongoing)
- One-sentence summary of what was delivered or requested

Also pull my inbox for the same period — flag any inbound emails where a partner seller
or end customer initiated contact unprompted (these are pull signals, not push).

After formatting, tell me:
- How many research reports or architecture assessments did I deliver? Did any produce
  a registered opportunity with an AE-owned follow-up?
- Are there threads where I sent something significant and there is no evidence of
  partner follow-through?
- Which inbound threads show genuine partner or customer interest without cold outreach?

Format as a markdown table.
```

---

## 2C — Format Your Email Log (manual — use if M365 MCP is not connected)

```
I need to format my email log for a client analytics tracker. Here are the key email threads I want to log:

[PASTE YOUR EMAIL THREAD SUMMARIES — date, subject, who it was with, what happened]

For each thread, I need:
- Date (YYYY-MM-DD)
- Subject / thread name
- Topic: bob / confluent / watsonx / security / renewal / research / event / marketing / pipeline
- Key contacts (name and/or role)
- Direction: Outbound / Inbound / Thread (ongoing)
- One-sentence summary of what happened or what was delivered

After formatting, tell me:
- How many research reports or architecture assessments did I deliver? Did any of them produce
  a registered opportunity with an AE-owned follow-up?
- Are there threads where I sent something significant (report, demo invite, research) and there
  is no evidence of partner follow-through?
- Which inbound threads show genuine partner or customer interest without cold outreach?

Format as a markdown table.
```

---

## 2D — Format Your Contact List

```
I need to format my contact list for a partner tracker. Here are my key contacts at [PARTNER NAME]:

[PASTE YOUR CONTACT LIST — name, title, region, what you know about them]

For each contact, I need:
- Full name
- Title / role (be specific: Account Executive, Solutions Architect, CTO, VP Sales, etc.)
- Function: Technical or Sales or Sales Ops or Marketing Exec or Technical Exec or Business Dev
- Product focus (which IBM products are they associated with)
- Region (e.g. Northeast, Southwest, Southeast, Rockies, Global)
- Last touch (month + year)
- Engagement level: High / Med / Low
  - High = actively engaged, has shown up to sessions, owns pipeline, or is a named champion
  - Med = has been in meetings but hasn't taken independent action
  - Low = on paper but no real engagement
- Email (if you have it)
- One-line note

After formatting, tell me:
- Who are my top 3 champions — the people who are genuinely bought in?
- Who are the executives (CTO, VP Sales, RVP) and have they taken any independent IBM-related action?
- Are there any critical gaps — roles I haven't connected with who I should?

Format as a markdown table.
```

---

## What to do with the output

After running all four sub-prompts, you'll have four formatted markdown tables.
Keep them in a single Bob conversation thread — you'll paste all four into the Step 3 prompt.

**If you used the M365 live-pull prompts (2B-Live / 2C-Live):** The attendee classification
is already done. When you reach Step 3, you can ask Bob to also highlight in the meeting log
which meetings had **no Technologent attendees** (IBM-only meetings) vs. which had genuine
partner participation — this is useful context for the Step 4 diagnosis.
