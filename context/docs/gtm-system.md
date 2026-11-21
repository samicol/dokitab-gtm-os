# DokiTab Outbound GTM System

**Miro board (visual flowchart):** https://miro.com/app/board/uXjVELGJWPI=/

End-to-end design for DokiTab's signal-led outbound engine. Grounded in `context/icp/` and `context/docs/research/`. Tool choices default to the stack in `CLAUDE.md` [CLAUDE.md]; thresholds, cadences, and any step not stated in those files are marked `[INFERRED]`. Work in progress, validated with sales and leadership before build [signal-matrix.md].

Product in one line: offline-first healthcare OS, HMS and EMR (pharmacy POS, lab LIMS) for Nigeria/West Africa, free core tier plus a ₦30,000/month plan [company.md]. PLG wedge: the free tier lets a facility self-serve a workspace; outbound exists to put that free workspace in front of the right facility at the right trigger [company.md] `[INFERRED]`.

Beachhead: Lagos private multi-specialty hospitals and diagnostic centers that bill HMOs [icp.md]. Segments S1 to S5 and personas (Champion CMD/consultant, Economic Buyer board/owner, Influencer dept leads) are defined in `context/icp/` [icp.md], [personas.md], [signal-matrix.md].

Pipeline shape: Context OS governs everything, then Signal to Source to Enrich to Fit-check to Route to People to Contacts to Sequence to Reply to CRM, with a weekly loop back into the Context OS.

---

## Step 1: The Context OS

**What Happens:** `CLAUDE.md`, `context/docs/research/`, and `context/icp/` are the single source of truth that every prompt, score, and message reads from. Inputs: research facts (each sourced and dated) [company.md], ICP tiers and disqualifiers [icp.md], persona pains and angles [personas.md], signal definitions and four-test scores [signal-matrix.md]. Transformation: these files are injected as context into Clay AI/Claygent prompts, scoring rules, and copy generation so no rep or model invents facts. Output: governed prompts, a scoring rubric, and approved message angles. Rule enforced from `CLAUDE.md`: cite a source for every company fact, never invent numbers, no em dashes, ask before sending/publishing, log every change [CLAUDE.md].

**Tool(s) Used:** Git + Claude Code (version-controlled context), Clay/Claygent (reads context into column prompts), the `campaign-strategy` / `build-clay` / `campaign-copy` skills [CLAUDE.md].

**Decision Gates:**
- IF a claim has no citation in `context/docs/research/` THEN it cannot enter a prompt or message; ELSE it is usable with its `[file.md]` tag [CLAUDE.md].
- IF a number (bed count, rejection rate, price) is not in research THEN mark `[INFERRED]` or drop it; never fabricate [CLAUDE.md].
- IF research and a directory conflict (e.g. free-plan claim) THEN first-party dokitab.com wins and the conflict is noted [company.md].

---

## Step 2: Buying Signals (always-on)

**What Happens:** The seven signals in `signal-matrix.md` run on a schedule and drop triggered accounts into the pipeline. Priority build order: (1) HEFAMAA registration/accreditation, (2) HMO panel participation/onboarding, (3) hiring records/billing/EMR/lab roles, (4) new branch opening, (5) PCN premises/new pharmacy [signal-matrix.md]. Each run outputs a list of facilities plus the triggering signal, stamped with a detection date. Signal-first, not ICP-first: the signal is the trigger, ICP is the filter applied after [workflow-anatomy.md], [signal-matrix-framework.md].

**Tool(s) Used:** n8n or scheduled-task cron (scheduler), crawl4ai (`tools/crawl4ai`) and Clay HTTP for registry/HMO/PCN scrapes, Apify/Clay "Find Jobs" for hiring, Google Places via Clay HTTP for branch/density [signal-matrix.md], [CLAUDE.md].

**Decision Gates:**
- IF signal detected AND account not seen in last 90 days THEN enqueue for sourcing; ELSE suppress (dedup) [workflow-anatomy.md].
- IF a scrape source returns empty or breaks THEN fall back to the next-ranked signal and flag the source as untested `[INFERRED]`; signal-matrix lists findability as asserted, not yet proven [signal-matrix.md].
- Run cadence by velocity `[INFERRED]`: hiring weekly, HEFAMAA/HMO/PCN registries monthly, branch openings bi-weekly [workflow-anatomy.md].

---

## Step 3: Finding Companies

**What Happens:** Build the raw facility universe for each segment. Inputs: segment definitions [icp.md]. Sources: HEFAMAA online registry (Lagos facilities, public QR verification since Dec 2025) [market.md], [signals.md]; Google Maps/Places for hospitals, labs, pharmacies, dental/eye/physio/maternity clinics [signal-matrix.md]; HMO and ILERA EKO accredited-provider directories [company.md], [market.md]; PCN premises lists for pharmacies [market.md]. Output: a deduped company table with name, facility type, location, and source signal.

**Tool(s) Used:** Google Maps/Apify (location scraping), crawl4ai + Clay HTTP (registry/directory scraping), Clay table as the store [signal-matrix.md], [CLAUDE.md].

**Decision Gates:**
- IF facility outside Nigeria THEN drop (out of profile); IF outside Lagos THEN tag Secondary and deprioritize vs the Lagos beachhead [icp.md].
- IF the same facility appears from two sources THEN merge on name + address, keep all source signals [workflow-anatomy.md].
- IF a source yields < expected volume (e.g. branch-opening news) THEN treat as a compound layer, not a standalone list [signal-matrix.md].

---

## Step 4: Enrichment (waterfall)

**What Happens:** Layer firmographic, technographic, and facility-verification data onto each account, cheap sources first, paid only on survivors. Firmographic: facility type, bed band (50 to 500+ for hospitals) [icp.md], branch count, ownership (private vs NGO) [company.md]. Technographic: current record state and incumbent software, confirmed on discovery not list-filterable (paper, legacy per-seat EMR, or open-source) [icp.md], [voice.md]. Facility verification: HEFAMAA/PCN registration status, HMO panels, website/domain existence [market.md], [company.md]. Output: an enriched row ready for scoring.

**Tool(s) Used:** Clay waterfall (Claygent for web facts, Apollo/Prospeo for firmographic), crawl4ai for registry verification, `rex` Clay tooling for cost control (`tools/rex`) [CLAUDE.md], [signal-matrix.md].

**Decision Gates:**
- IF a cheap source resolves a field THEN stop the waterfall for that field (no paid call); ELSE escalate to the next provider [signal-matrix-framework.md] `[INFERRED]`.
- IF HEFAMAA/PCN registration cannot be verified THEN flag "unverified facility" and hold from paid enrichment `[INFERRED]`.
- IF no website/custom domain found THEN tag `no-domain` and route contact discovery to the phone/WhatsApp path (Step 8) `[INFERRED]`.

---

## Step 5: The ICP Fit Check

**What Happens:** Automated qualification against `icp.md`. Each account is scored on firmographic fit, intent-signal strength, and findability, then passed, held, or disqualified. Scoring model `[INFERRED]` (no rubric exists in the files): +3 Lagos multi-specialty hospital/diagnostics (Tier A), +2 Tier B standalone lab/pharmacy/specialty clinic, 0 Tier C; +2 strong signal (HEFAMAA new-reg, HMO onboarding), +1 hiring/branch; +1 HMO-billing confirmed. Qualify at >= 4, hold 2 to 3, disqualify < 2 `[INFERRED]`. Thresholds tie to the `icp.md` tiering [icp.md].

**Tool(s) Used:** Clay formula + Claygent scoring column reading the Context OS rubric, `campaign-qa` skill for table QA [CLAUDE.md].

**Decision Gates (disqualification gates from `icp.md`):**
- IF outside Nigeria (or unproven West Africa) THEN disqualify [icp.md].
- IF needs air-gapped no-sync, OR requires a proven track record / formal HIPAA certification before buying THEN disqualify (DokiTab cannot clear these today) [icp.md].
- IF needs AI scribe/telemedicine/multi-branch at zero cost THEN disqualify or reroute to paid-plan messaging [icp.md].
- IF score >= 4 THEN route (Step 6); ELSE IF 2 to 3 THEN hold for more enrichment; ELSE drop [icp.md] `[INFERRED]`.

---

## Step 6: Routing by Segment

**What Happens:** Qualified accounts split by vertical, size, and primary signal so each gets the right angle. Routes map to S1 to S5 and to the triggering signal [signal-matrix.md]. Output: each account lands in one workflow lane (segment x primary signal) with its persona angle attached from Matrix 1 [signal-matrix.md].

**Tool(s) Used:** Clay routing/conditional columns, n8n for lane dispatch [CLAUDE.md], [workflow-anatomy.md].

**Decision Gates:**
- IF multi-specialty hospital + diagnostics THEN S1 lane (full-OS angle); IF standalone lab THEN S2 (LIMS/PACS angle); IF pharmacy THEN S3 (FEFO/PCN angle); IF single-specialty clinic THEN S4 (5-minute standalone angle); IF teaching/NGO THEN S5 (deprioritized, registry angle) [signal-matrix.md], [icp.md].
- IF account carries two signals THEN route by the higher-scoring signal, stack the second as a secondary enrichment for copy [workflow-anatomy.md].
- IF 50+ beds or multi-branch THEN flag for Economic-Buyer-led motion (board/TCO angle); ELSE Champion-led [personas.md] `[INFERRED]`.

---

## Step 7: Finding People

**What Happens:** Identify the buying group per account. Champion: CMD / consultant [personas.md]. Economic Buyer: board / owner/proprietor (`[INFERRED FROM RESEARCH]`) [personas.md]. Influencers: pharmacist, lab scientist, billing/claims officer, records officer, IT/EMR admin, nurse, cashier [personas.md]. Output: named contacts with title and persona tag per account.

**Tool(s) Used:** Apollo/Prospeo (people search), Clay "Find People", LinkedIn/Sales Navigator, Claygent for site "our team" pages [workflow-anatomy.md], [CLAUDE.md].

**Decision Gates:**
- IF CMD/Medical Director found THEN set as primary Champion; ELSE fall back to any consultant or the facility admin `[INFERRED]`.
- IF S1/hospital THEN also find the Economic Buyer (owner/board) for a parallel TCO thread [personas.md] `[INFERRED]`.
- IF standalone pharmacy/lab/clinic (S2 to S4) THEN owner is often also the Champion; collapse to a single contact `[INFERRED]`.
- IF no decision-maker resolves THEN hold account (do not sequence a generic inbox) `[INFERRED]`.

---

## Step 8: Finding and Verifying Emails & Phone/WhatsApp

**What Happens:** Build a contact waterfall per person, with a Nigeria-specific fallback because many clinics lack custom domains. Email: find, then verify, then catch-all handling. Phone/WhatsApp: the primary fallback and often the primary channel locally [company.md] `[INFERRED]`. Output: a reachable channel per contact with a confidence tag.

**Tool(s) Used:** Apollo/Prospeo + Clay email waterfall, an email verifier (e.g. ZeroBounce/NeverBounce) `[INFERRED]`, WhatsApp Business for messaging, phone enrichment via Clay/Apollo [CLAUDE.md], [workflow-anatomy.md].

**Decision Gates:**
- IF verified business email found THEN primary = email; ELSE continue waterfall.
- IF only a catch-all domain THEN mark risky, cap send volume, and pair with WhatsApp `[INFERRED]`.
- IF no custom domain (facility uses Gmail/Yahoo or none) THEN primary = WhatsApp/phone, email becomes secondary `[INFERRED]`.
- IF no email and no phone THEN return account to sourcing as under-enriched `[INFERRED]`.
- Privacy: never put patient or personal data in any payload; WhatsApp/SMS add-ons are DokiTab's own channel, outreach uses business contacts only [company.md], [CLAUDE.md].

---

## Step 9: The Sequence (Email, LinkedIn, WhatsApp/Phone Handoff)

**What Happens:** Multi-channel cadence carrying the segment x persona angle from Matrix 1 [signal-matrix.md], with two CTA modes: low-friction PLG (start a free workspace) and demo (book a live demo / WhatsApp) [company.md]. Cadence `[INFERRED]`: Day 1 email (signal-referenced), Day 2 LinkedIn connect/view, Day 4 email bump, Day 6 WhatsApp touch, Day 9 breakup; the free-tier PLG CTA lowers friction so many accounts self-start before a call [company.md] `[INFERRED]`.

**Tool(s) Used:** Smartlead/Instantly (email) `[INFERRED]`, LinkedIn/Sales Navigator, WhatsApp Business, `campaign-copy` skill for the copy, `inbox-placement-test` skill for deliverability [CLAUDE.md].

**Decision Gates:**
- IF Champion (CMD) THEN lead with claim-rejection + offline angle, CTA = book demo or start free [customers.md], [voice.md].
- IF Economic Buyer THEN lead with no per-seat/no server TCO angle, CTA = board-ready demo [voice.md], [company.md].
- IF Influencer THEN lead with role-specific pain (FEFO, panic-values, clean claims), CTA = start free workspace [company.md].
- IF email primary = WhatsApp (no-domain account) THEN skip cold email, open on WhatsApp with a short local-language-friendly note `[INFERRED]`.
- IF no reply after full cadence THEN drop to nurture, allow re-entry on a new signal [workflow-anatomy.md].

---

## Step 10: Reply Handling

**What Happens:** Classify every reply by intent and route it, with objection playbooks for the two predictable doubts: legacy EMR lock-in and offline/power reliability. Output: each reply tagged (positive / objection / referral / not-now / negative) and routed to the right next action and CRM stage.

**Tool(s) Used:** Claygent/LLM intent classifier on the inbox, HubSpot for routing, human rep for positive/complex [CLAUDE.md] `[INFERRED]`.

**Decision Gates + objection playbooks:**
- IF positive/demo-interest THEN book demo or confirm workspace signup, move stage, hand to rep [company.md].
- IF objection = "locked into our current EMR" THEN counter: free core tier and no per-seat fees let them run DokiTab in parallel on one department first, no rip-and-replace [company.md], [voice.md].
- IF objection = "offline/power doubts" THEN counter: offline-first SQLite, vitals/MAR/SOAP held locally and replayed on reconnect through NEPA cuts [company.md], [voice.md].
- IF objection = "is it secure / where is our data" THEN counter: AWS Lagos in-country residency, AES-256 offline queue, immutable audit, NDPA 2023 alignment [company.md], [market.md].
- IF objection = "unproven vendor" THEN counter: start free, no lock-in, verify before spending [icp.md] `[INFERRED]`.
- IF referral THEN capture new contact, re-enter at Step 7.
- IF negative/opt-out THEN suppress and log reason to the objection log (Step 12).

---

## Step 11: The CRM

**What Happens:** Objects, stages, and attribution, with PLG workspace signups synced alongside outbound. Object model `[INFERRED]`: Account (facility) with facility_type, segment S1 to S5, tier, HEFAMAA/HMO status; Contact with persona tag; Opportunity with plan intent (free vs ₦30k) [company.md]. Attribution fields: source_signal, detection_date, segment, angle, channel. PLG sync: a free-tier workspace signup creates/updates the Account with an activation flag so sales sees self-serve adoption next to outbound touches [company.md] `[INFERRED]`.

**Tool(s) Used:** HubSpot (CRM, named stack) [CLAUDE.md], n8n to sync DokiTab workspace-signup events into HubSpot `[INFERRED]`.

**Pipeline stages `[INFERRED]`:** Signal Detected to Enriched/Qualified to Contacted to Engaged/Replied to Workspace Activated (PLG) to Demo to Paid (₦30k) to Lost.

**Decision Gates:**
- IF workspace signup event arrives for a known Account THEN mark Workspace Activated and alert the rep; ELSE create a new PLG-sourced Account [company.md] `[INFERRED]`.
- IF Activated but inactive after N days THEN trigger an activation nurture `[INFERRED]`.
- IF Opportunity reaches Paid THEN record which signal/segment/angle attributed it (feeds Step 12).

---

## Step 12: The Weekly Feedback Loop

**What Happens:** Weekly, outbound outcomes update the Context OS. Inputs: reply rates by segment/angle, bounce rates by source, the objection log, and PLG workspace activations. Transformation: compare outcomes to the four-test scores and ICP tiers. Output: concrete edits to `icp.md` (tier/disqualifier changes), `personas.md` (pain wording, objection counters), and `signal-matrix.md` (re-score or kill signals, confirm findability). Logged in `work-log/` per the `CLAUDE.md` rule [CLAUDE.md].

**Tool(s) Used:** HubSpot reports + Smartlead/Instantly stats into a weekly review, Claude Code to edit the context files, Git to version them [CLAUDE.md].

**Decision Gates:**
- IF a signal's real reply rate is low across enough volume THEN downgrade or kill it in `signal-matrix.md` and document why (as the framework kills dead signals) [signal-matrix-framework.md].
- IF a segment converts well to workspace activations THEN raise its ICP tier in `icp.md`; IF it never converts THEN move it toward disqualification [icp.md].
- IF an objection recurs THEN add/upgrade its counter in `personas.md` and Step 10 [personas.md].
- IF bounce rate high from a source THEN tighten verification in Step 8 before re-running [workflow-anatomy.md].
- IF HMO claim-rejection proxy is found to work THEN promote it from Open Question to an active signal [icp.md], [signal-matrix.md].

---

## Final Audit: The Four Pillars

| Pillar | How the system enforces it | #1 Nigerian healthcare failure mode it prevents | Metric that proves it works |
|---|---|---|---|
| Clean Data | Context OS citation rule (no fact without a `[file.md]` source, never invent numbers) [CLAUDE.md]; waterfall enrichment with HEFAMAA/PCN verification before any paid call (Steps 4 to 5) | Chasing unregistered, mislabeled, or phantom facilities, and messaging on invented bed/HMO numbers `[INFERRED]` | Verified-facility rate (% of sourced accounts with confirmed HEFAMAA/PCN status) and bounce rate < target `[INFERRED]` |
| Always-On Signals | Scheduled signal runs feeding the pipeline, signal-first then ICP filter (Step 2), priority order from `signal-matrix.md` | Static, one-time list buys that ignore the real trigger (new registration, HMO onboarding, inspection) [signal-matrix.md] | Signals live and firing weekly; % of pipeline that is signal-triggered vs cold `[INFERRED]` |
| Composable Workflows | Universal signals built once and replicated across S1 to S5 by keyword/angle swap; one Clay build, many lanes (Steps 2, 6, 9) [signal-matrix.md], [signal-matrix-framework.md] | One-off, non-repeatable manual campaigns per clinic type that never scale [signal-matrix-framework.md] | Ratio of universal to niche-specific signals live (target majority universal) [signal-matrix.md] |
| Tight Feedback Loops | Weekly loop writes reply/bounce/objection/activation data back into `icp.md`, `personas.md`, `signal-matrix.md`, logged in Git (Step 12) [CLAUDE.md] | ICP, personas, and signals going stale while reps keep running dead angles [signal-matrix-framework.md] | Cadence of context-file updates per week and signal win-rate trend after re-scoring `[INFERRED]` |

## Open gaps (engineering-ready caveats)
- Scrape reliability for HEFAMAA/HMO/PCN sources is asserted from research, not yet tested [signal-matrix.md].
- Scoring thresholds, cadences, CRM object model, and the sending platform are `[INFERRED]`; confirm with sales before build.
- Account-level HMO claim-rejection rate (the sharpest pain) still has no external proxy [icp.md].
- No Matrix 3 workflow tracker yet; generate once priority signals are confirmed [signal-matrix.md].
