# Prompt 01 — Research Your Partner

**Mode:** IBM VAR Channel Strategist  
**MCP tools needed:** Tavily, IBM Data & AI Docs  
**When to run:** Before you touch any data. This gives you the strategic context for everything else.

---

## How to use this prompt

1. Switch Bob to **IBM VAR Channel Strategist** mode
2. Copy the prompt below
3. Replace every `[BRACKETED]` placeholder with your actual values
4. Paste into Bob and run

---

## The Prompt

```
I am an IBM Partner Technical Specialist (PTS) and I need to deeply understand my Business Partner
before building a partnership tracker.

My partner is: [PARTNER NAME]
Their IBM tier (if known): [Silver / Gold / Platinum — or "unknown"]
Their distributor (if known): [Arrow / Ingram Micro / Direct / unknown]
Their HQ location: [CITY, STATE]
Their primary industries/verticals: [e.g. Financial Services, Healthcare, Manufacturing — or "unknown"]

Please research this partner using Tavily and tell me:

1. What IBM tier are they, and what does that mean for margins and commitments?
2. What is their distributor relationship — do they buy through Arrow or another VAD, or direct?
3. What IBM products are they known to sell or resell? (check IBM PartnerWorld directory if possible)
4. What industries and accounts do they specialize in?
5. What other vendors do they carry? (this tells me where IBM competes for their attention)
6. Are they WBENC, MWBE, SDVOSB, or any other certified diversity supplier?
7. Any recent news — acquisitions, leadership changes, new offices, new practices?
8. Based on what you find: what IBM Data & AI products are the strongest fit for their customer base
   and sales motion? Pull from the IBM Data & AI Docs MCP to ground this.

After your research, give me a one-paragraph "partner profile" I can use as the opening context
for my tracker — describing who they are, what their IBM relationship looks like, and what the
biggest opportunity is for growing IBM Data & AI revenue through them.

Finally, tell me honestly: based on what you can see publicly, does this partner look like a
VAR that has committed to IBM Data & AI as a practice, or are they a broad multi-vendor reseller
where IBM is one of many lines? What would you need to see to believe they've truly bought in?
```

---

## What to do with the output

Save the one-paragraph partner profile — you'll paste it into the tracker's Dashboard tab in Step 3.

Keep the "have they bought in?" answer. It sets the honest baseline for the "What's Working"
analysis in Step 4. If the research already shows signs of shallow commitment (no IBM badges,
no published IBM case studies, IBM buried in a long vendor list), that context matters when
you're interpreting your own pipeline data.

---

## Follow-up prompt (if the partner has multiple regions or practice areas)

```
[PARTNER NAME] has [X] regional offices / practice areas. The ones most relevant to IBM Data & AI are:
[LIST OFFICES OR PRACTICE LEADS IF KNOWN]

Which regions or practice areas should I prioritize for IBM engagement? Use what you know about
IBM's VAR channel motion — I want to focus where the partner has both customer access and at least
some technical readiness to demo and sell independently.
```
