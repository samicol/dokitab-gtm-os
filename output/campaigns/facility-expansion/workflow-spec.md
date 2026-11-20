# Workflow Spec: New Facility (facility-expansion)

**Status:** STRUCTURE-APPROVED 2026-11-20 · copy pending

> Interpretation (confirm at Checkpoint 1): this is DokiTab's #1 workflow (WF1, HEFAMAA accreditation, score 17) [signal-matrix.md], framed as "New facility." Primary trigger = HEFAMAA new registration/accreditation (a facility newly entering or re-accrediting in the registry). New-branch/opening (signal 4) is folded in as a secondary trigger since both mean a new or expanding facility. Change this split if you intended new-branch-opening as primary.
>
> Facts note: `context/messaging/facts.md` does not exist yet, so specific product facts below are cited from `context/docs/research/` (authorized context). Any item not in those files is `{{placeholder}}` or `[INFERRED]`.

---

## 1. Mother Intent (Trigger) [ENGINE]

A Nigerian healthcare facility that newly registered or re-accredited with HEFAMAA, or opened a new site/branch, within the last ~90 days [INFERRED window]. Catalog match: "Regulatory changes" plus "Recently Incorporated Businesses" / expansion [workflow-anatomy.md], and it is the top-ranked signal in `signal-matrix.md` (HEFAMAA, score 17; new branch, score 15) [signal-matrix.md]. Expensive-to-fake: a registry entry and a physical branch cost money and clear an inspection, unlike a marketing claim [signal-matrix-framework.md].

## 2. Inclusion / Exclusion [ICP]

**Include:**
- ICP box [icp.md]: Primary Lagos private multi-specialty hospitals and diagnostic centers (Tier A beachhead); also Tier B standalone diagnostic labs/PACS, pharmacies, and single-specialty clinics (dental, eye, physio, maternity). Hospitals 50 to 500+ beds; standalone facilities have no bed band. Geography: Nigeria, Lagos first [icp.md].
- Trigger definition: a new HEFAMAA registry entry with registration/accreditation date within ~90 days [market.md], [signals.md]; OR a public new-branch/opening announcement within ~90 days [signal-matrix.md]. [INFERRED] 90-day recency.
- Recipient [personas.md]: Champion = CMD / consultant; Economic Buyer = owner / board (hospitals); for standalone Tier B facilities the owner is often also the Champion, so collapse to one contact [personas.md], [gtm-system.md].

**Exclude:**
- Outside Nigeria; wider West Africa deprioritized as unproven [icp.md].
- Tier C teaching hospitals and donor/NGO programs: long procurement cycle, no reference [icp.md].
- Disqualifiers [icp.md]: needs air-gapped no-sync; requires a proven track record or formal HIPAA certification before buying; wants AI scribe/telemedicine/multi-branch at zero cost (reroute to paid-plan messaging, do not cold-trigger).
- Existing DokiTab workspaces, accounts in active pipeline, and competitors [gtm-system.md] [INFERRED].
- Unverified facilities (no HEFAMAA/PCN confirmation): hold, do not enrich paid [gtm-system.md].

## 3. Sources [ENGINE]

- **Trigger source:** HEFAMAA online registry (hefamaaportal.com.ng), public QR verification live since Dec 2025, scraped via crawl4ai (`tools/crawl4ai`) or Clay HTTP [signal-matrix.md], [signals.md], [market.md]. Secondary trigger: new-branch via Google Places/Maps new-listing delta plus press (Nairametrics, Guardian) via crawl4ai [signal-matrix.md].
- **Enrichment sources:** Google Places (facility type, location) [signal-matrix.md]; Claygent (website/custom-domain check, record-state discovery) [gtm-system.md]; HMO and LASHMA/ILERA EKO accredited-provider directories (HMO participation) [company.md], [market.md]; PCN premises lists (pharmacies) [market.md]; Apollo/Prospeo (firmographic + people) [gtm-system.md].

## 4. Signals Table [ENGINE]

| # | Signal | Role | How it's read (boolean / count / regex / enum) | Output |
|---|---|---|---|---|
| 1 | HEFAMAA new registration/accreditation | trigger | boolean: registry entry with reg/accreditation date <= 90 days [market.md], [signals.md] | `is_new_hefamaa` + `reg_date` |
| 2 | New branch / new site opening | trigger (secondary) | boolean: Google Places new listing or press mention <= 90 days [signal-matrix.md] [INFERRED detection] | `is_new_branch` |
| 3 | Facility vertical | router | AI/Places enum: `multi_specialty_hospital` / `diagnostic_lab` / `pharmacy` / `specialty_clinic` / `teaching_or_ngo` [icp.md], [signal-matrix.md] | `segment` (S1 to S5) |
| 4 | Hospital size band | router | count band (hospitals only): 50-150 / 150-500 / 500+ ; n/a for standalone [icp.md] | `size_band` |
| 5 | HMO panel participation | router | boolean: listed on >= 1 HMO/ILERA EKO provider directory [company.md], [market.md] | `bills_hmo` |
| 6 | Custom domain present | router | regex: has corporate email domain (not gmail/yahoo/none) [gtm-system.md] | `has_domain` (email-first vs WhatsApp-first path) |
| 7 | PCN premises registration (pharmacies) | router (S3 only) | boolean: on PCN premises list [market.md] | `pcn_registered` |
| 8 | Incumbent record state | context-clause (NEVER routes) | AI enum, discovery-confirmed not list-filterable: `paper` / `legacy_emr` / `open_source` / `unknown` [icp.md], [voice.md] | `record_state` (copy color only) |

**Count clusters (signal 4, `size_band`)** [INFERRED banding within the cited 50 to 500+ range [icp.md]]:
- 50-150 beds = small hospital (owner-led, faster decision).
- 150-500 beds = mid hospital (Champion + Economic Buyer both engaged).
- 500+ beds = large/teaching-adjacent (longer cycle, board-led).

## Qualification Rules (ICP fit gate) [ICP]

Applies the `gtm-system.md` Step 5 fit check [gtm-system.md]; thresholds `[INFERRED]` (no rubric exists in the files):
- IF `is_new_hefamaa` OR `is_new_branch` is false THEN not triggered, drop.
- IF `segment` in {S1, S2, S3, S4} THEN proceed; IF `teaching_or_ngo` (S5) THEN hold/deprioritize [icp.md].
- Score: +3 S1 Lagos multi-specialty/diagnostics, +2 Tier B (S2/S3/S4); +2 `is_new_hefamaa`, +1 `is_new_branch`; +1 `bills_hmo`. Qualify at >= 4, hold 2 to 3, drop < 2 [INFERRED].
- Hard disqualify gates from Exclusion above override any score [icp.md].
- IF HEFAMAA/PCN status unverifiable THEN hold before paid enrichment [gtm-system.md].

---

## 5. Routing Matrix with Angles (the Angle Matrix) [ENGINE]

Routing axes (as named): `segment` (router) x persona/recipient, modulated by `bills_hmo` and `size_band`. `has_domain` routes channel, not narrative (handled in Step 8). Angles are meaning, not copy; `/campaign-copy` turns them into lines [workflow-anatomy.md].

**Meta-angle (applies to every cell, this is the niche-native hook):** a new or newly-accredited facility has no incumbent system to rip out and has not set paper habits yet, so the pitch is greenfield: start on the right record from day one, free, instead of migrating later [company.md], [voice.md], [icp.md]. This is what makes the angle specific to "new facility" and not generic EMR outreach.

Axis counts (flat): 4 in-scope segments (S1 to S4) x 3 personas = 12 cells. For standalone Tier B facilities (S2 to S4) the owner is frequently also the Champion, so those two rows collapse to one contact in execution [personas.md], [gtm-system.md].

| Cell (segment x persona) | Deterministic read | Inferred situation | Angle it earns |
|---|---|---|---|
| S1 x Champion (CMD) | `segment=S1` + trigger + recipient CMD | Standing up/expanding a multi-specialty hospital; picking clinical systems now | One offline-first record so charting survives NEPA cuts and HMO claims are clean from the first encounter, no later migration [voice.md], [company.md], [customers.md] |
| S1 x Economic Buyer (owner/board) | `segment=S1` + recipient owner/board | New-build capital is tight; legacy adds server room + per-seat fees | Run the whole new hospital (wards, theatre, accounting, HMO claims) free, no server room, no per-seat fees; keep capital for clinical kit [voice.md], [company.md] |
| S1 x Influencer (billing/claims, lab) | `segment=S1` + recipient dept lead | New ops team setting up billing and lab workflows | Set up clean HMO claim submission and lab panic-value/HL7 feeds on day one instead of inheriting a paper backlog [company.md] |
| S2 x Champion (lab owner) | `segment=S2` + trigger | New standalone lab choosing a LIMS; analyzers to connect | Launch with digital results, Mindray/Sysmex/Roche HL7 feeds, barcode tracking, ISO 15189-aligned records, no server [company.md] |
| S2 x Economic Buyer | `segment=S2` + recipient owner | New lab watching cost as volume scales | Free LIMS plus PACS, no per-seat licensing as test volume grows [company.md] |
| S2 x Influencer (lab scientist) | `segment=S2` + recipient scientist | Bench team setting up result flow | SMS panic-value alerts and home-phlebotomy dispatch from launch, results off paper [company.md], [voice.md] |
| S3 x Champion (superintendent pharmacist) | `segment=S3` + trigger + `pcn_registered` | New pharmacy premises, PCN inspection ahead | Open PCN-ready with a controlled-drugs register and FEFO dispensing that keep running through outages [company.md], [market.md] |
| S3 x Economic Buyer (owner) | `segment=S3` + recipient owner | New retail/hospital pharmacy, thin margins | Free multi-till POS from day one; stop expiry write-offs and cashier leakage before they start [company.md], [voice.md] |
| S3 x Influencer (pharmacist/cashier) | `segment=S3` + recipient staff | Counter staff setting up dispensing/till | FEFO batch dispensing with expiry/reorder alerts and split-tender POS that reconciles each shift [company.md] |
| S4 x Champion (consultant) | `segment=S4` + trigger + `specialty` | New single-specialty clinic opening | 5-minute standalone mode tuned to the specialty (odontogram / Snellen-IOP / ROM-MMT / partograph), ward screens hidden, free to start [company.md] |
| S4 x Economic Buyer (owner) | `segment=S4` + recipient owner | Owner-operator funding the fit-out | Start free, no server, no per-seat fees; pay only if you later want AI scribe or telemedicine [company.md] |
| S4 x Influencer (nurse/front desk) | `segment=S4` + recipient staff | Front-of-house setting up charting | BYOD-safe charting from launch, biometric lock and offline sync, no paper folders [voice.md], [company.md] |

**Modulator routing (declared):**
- `bills_hmo`: IF true THEN lead the cell with HMO claim-rejection prevention (DokiTab's one proven outcome) [customers.md]; ELSE lead with compliance/greenfield records [icp.md]. New-HEFAMAA-only facilities are often pre-HMO, so they take the compliance lead; new branches of existing groups usually take the HMO lead [INFERRED].
- `size_band`: IF 50-150 or standalone THEN owner-led, low-friction free-workspace PLG CTA; IF 150-500 or 500+ THEN Champion + Economic Buyer dual thread, demo CTA [gtm-system.md], [personas.md] [INFERRED].

**Branch coverage:** no cell references a signal absent in it. IF `bills_hmo=false` no claim-rejection line (compliance instead). IF trigger is `is_new_branch` not HEFAMAA, `reg_date` is absent so use opening recency. `record_state` is context-clause color only and never the routing reason [icp.md].

## 6. Variables Contract [ENGINE]

| Variable | Source field | Exists in branches |
|---|---|---|
| `{{facility_name}}` | Google Places / HEFAMAA | all |
| `{{facility_type}}` | signal 3 enum | all |
| `{{city_area}}` | Google Places | all |
| `{{reg_date}}` | HEFAMAA registry | HEFAMAA-trigger branches only |
| `{{opening_ref}}` | Places / press | new-branch branches only |
| `{{hmo_name}}` | HMO/ILERA EKO directory | `bills_hmo=true` only |
| `{{specialty}}` | signal 3 (S4) | S4 only |
| `{{first_name}}` / `{{title}}` | Apollo / Prospeo | all contacted |

Data-must-exist: any fact not delivered by enrichment cannot appear in copy. Specific product figures stay `{{placeholder}}` until `context/messaging/facts.md` exists [INFERRED].

## 7. Cadence & Channel [ENGINE]

- Periodicity: always-on. Run the HEFAMAA registry scrape monthly and the new-branch scrape bi-weekly, matching signal velocity [workflow-anatomy.md], [signal-matrix.md] [INFERRED].
- Channel: email + LinkedIn for facilities with a custom domain; WhatsApp/phone-first for `has_domain=false` facilities [gtm-system.md].
- Dedup: suppress 90 days after outreach; allow re-entry on a new signal [workflow-anatomy.md].

---

## 8. Clay Enrichment Steps [ENGINE]

Ordered waterfall build, cheap source first, paid only on survivors; prove each paid column on one qualifying and one rejecting row before scaling [build-clay], [gtm-system.md]. Credit counts `[INFERRED]`.

| Step | Clay build | Input to output |
|---|---|---|
| 1 | Source import: HEFAMAA registry rows (crawl4ai) + Google Places new listings into the Company table | raw rows to `facility_name`, `address`, `city_area`, `reg_date`/`opening_ref`, `source_signal` [signal-matrix.md] |
| 2 | Vertical classification (signal 3): Claygent AI enum | website/listing to `segment` S1 to S5 [icp.md] |
| 3 | Facility verification: confirm HEFAMAA status; PCN premises for S3 | registry match to verified flag; hold if unverifiable [gtm-system.md] |
| 4 | Size band (signal 4, hospitals): Apollo/Places | headcount/beds to `size_band` clusters [icp.md] |
| 5 | HMO panel check (signal 5): scrape HMO/ILERA EKO directories | match to `bills_hmo` + `{{hmo_name}}` [company.md], [market.md] |
| 6 | Domain check (signal 6): Claygent regex on website | email domain to `has_domain` [gtm-system.md] |
| 7 | Record-state discovery (signal 8, context-clause): AI enum | to `record_state` (copy color only) [icp.md], [voice.md] |
| 8 | ICP fit score + gate: Clay formula (Step 5 rubric) | signals to qualify / hold / drop [icp.md], [gtm-system.md] |
| 9 | People: Apollo/Prospeo + Clay Find People | to Champion / Economic Buyer / Influencer per segment [personas.md] |
| 10 | Contact waterfall: email find + verify, catch-all handling, phone/WhatsApp fallback for no-domain | to `{{email}}` + channel + confidence [gtm-system.md] |

Gates mirror `gtm-system.md` Steps 4 to 8: cheap-source-resolves stops the waterfall; unverified facility holds from paid enrichment; no-domain routes to WhatsApp/phone; no email and no phone returns the account to sourcing [gtm-system.md].

## 9. Sequence Structure and Copy Handoff [ENGINE]

This skill writes no copy. Below is the sequence STRUCTURE only; `/campaign-copy` reads the Angle Matrix (section 5) and writes the lines under its line laws, gated on this file's `STRUCTURE-APPROVED` status.

- Channels: email + LinkedIn for `has_domain=true`; WhatsApp/phone-first for `has_domain=false` [gtm-system.md].
- Cadence `[INFERRED]` (from `gtm-system.md` Step 9): Day 1 email (greenfield angle, signal-referenced), Day 2 LinkedIn connect/view, Day 4 email bump, Day 6 WhatsApp touch, Day 9 breakup. No-domain path: open on WhatsApp Day 1, skip cold email.
- Each step carries the cell angle from section 5, modulated by `bills_hmo` and `size_band`.
- CTA mode by `size_band`: small/standalone to "start a free workspace" (PLG, fastest activation); mid/large to "book a live demo" [company.md], [gtm-system.md].
- Handoff: run `/campaign-copy` on this file; a copy flaw is fixed here in the spec and re-approved, never patched inline.

## 10. Reply-Handling Rules [ENGINE]

Intent classification then routing, with the objection counters from `gtm-system.md` Step 10. Tool: Claygent/LLM classifier plus HubSpot routing [gtm-system.md].

- IF positive/demo-interest THEN book demo or confirm workspace signup, advance CRM stage, hand to rep [company.md].
- IF objection "locked into our current EMR" THEN counter: free core and no per-seat fees let them run DokiTab in parallel on one department first, no rip-and-replace [company.md], [voice.md].
- IF objection "offline/power doubts" THEN counter: offline-first SQLite, vitals/MAR/SOAP held locally and replayed on reconnect through NEPA cuts [company.md], [voice.md].
- IF objection "where is our data / is it secure" THEN counter: AWS Lagos in-country residency, AES-256 offline queue, immutable audit, NDPA 2023 alignment [company.md], [market.md].
- IF objection "unproven vendor" THEN counter: start free, no lock-in, verify before spending [icp.md] `[INFERRED]`.
- IF referral THEN capture the new contact and re-enter at Step 9 (People).
- IF not-now THEN nurture, allow re-entry on a new signal [workflow-anatomy.md].
- IF negative/opt-out THEN suppress and log the reason to the objection log, which feeds the weekly loop [gtm-system.md].

---

**Structure complete and approved.** Definition of done met: mother intent, inclusion/exclusion, sources, signals table (roles + clusters), angle matrix with every cell's angle, variables contract, cadence, enrichment steps, and reply-handling, all with Engine/ICP markers. Next: run `/campaign-copy` to write and QA the lines; it will advance the status to `COPY-APPROVED`. Any angle flaw found in copy is fixed here first.
