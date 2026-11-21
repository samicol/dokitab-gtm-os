# Workflow Spec: New Facility (facility-expansion)

**Status:** COPY-APPROVED 2026-11-21 (S1 x CMD cell; email + LinkedIn) · copy in [copy.md](copy.md)

> Revised 2026-11-21 for Clay buildability: the trigger is a Google Places monthly snapshot delta (new facility listings, keyed by `place_id`) plus dated public opening announcements (news / LinkedIn / Instagram / Facebook). HEFAMAA is NOT part of the Clay build: the registry is login-gated with per-facility QR lookup only, and registration is legally required of every Lagos facility (so near-universal), so it is confirmed manually at the demo stage, not used to trigger, gate, or score. The `copy.md` e1l1 waterfall routes on `is_new_opening` / `is_new_listing` to match this architecture.
>
> Facts note: `context/messaging/facts.md` does not exist yet, so specific product facts below are cited from `context/docs/research/` (authorized context). Any item not in those files is `{{placeholder}}` or `[INFERRED]`.

---

## 1. Mother Intent (Trigger) [ENGINE]

A Nigerian healthcare facility that first appears as a new Google Places listing (monthly snapshot delta, keyed by `place_id`), or is named in a public opening announcement (news / LinkedIn / Instagram), within the last ~90 days [INFERRED window]. HEFAMAA is not used in the Clay build (login-gated, per-facility QR only); registration is legally required of every Lagos facility, so it is assumed and confirmed manually at the demo stage, not used to trigger, gate, or score [signal-matrix.md]. Catalog match: "New product launches" / expansion plus "Recently Incorporated Businesses" [workflow-anatomy.md]; it maps to `signal-matrix.md` signals 1 and 4 [signal-matrix.md]. Expensive-to-fake: a physical premises and a public opening cost money, unlike a marketing claim [signal-matrix-framework.md].

## 2. Inclusion / Exclusion [ICP]

**Include:**
- ICP box [icp.md]: Primary Lagos private multi-specialty hospitals and diagnostic centers (Tier A beachhead); also Tier B standalone diagnostic labs/PACS, pharmacies, and single-specialty clinics (dental, eye, physio, maternity). Hospitals 50 to 500+ beds; standalone facilities have no bed band. Geography: Nigeria, Lagos first [icp.md].
- Trigger definition: a facility whose Google Places `place_id` first appears in the monthly snapshot within ~90 days (`is_new_listing`), OR a public opening announcement (news/LinkedIn/Instagram) within ~90 days (`is_new_opening`) [signal-matrix.md]. [INFERRED] 90-day recency. HEFAMAA registration is assumed (legally required in Lagos), confirmed manually at the demo stage, not used in Clay [market.md], [signals.md].
- Recipient [personas.md]: Champion = CMD / consultant; Economic Buyer = owner / board (hospitals); for standalone Tier B facilities the owner is often also the Champion, so collapse to one contact [personas.md], [gtm-system.md].

**Exclude:**
- Outside Nigeria; wider West Africa deprioritized as unproven [icp.md].
- Tier C teaching hospitals and donor/NGO programs: long procurement cycle, no reference [icp.md].
- Disqualifiers [icp.md]: needs air-gapped no-sync; requires a proven track record or formal HIPAA certification before buying; wants AI scribe/telemedicine/multi-branch at zero cost (reroute to paid-plan messaging, do not cold-trigger).
- Existing DokiTab workspaces, accounts in active pipeline, and competitors [gtm-system.md] [INFERRED].
- S3 pharmacies that fail PCN verification: hold that branch, do not enrich paid. HEFAMAA registration is assumed (legally required in Lagos) and confirmed manually at the demo stage, not in Clay [gtm-system.md].

## 3. Sources [ENGINE]

- **Trigger source:** Google Places monthly snapshot of Nigerian healthcare facilities, keyed by `place_id`, diffed against a persisted baseline table to output `first_seen_date` and `is_new_listing` [signal-matrix.md]. Mechanism: the baseline is a Google Sheet / n8n store keyed by `place_id`; `is_new_listing` is computed as `place_id NOT IN baseline` (a Clay lookup column plus formula, or n8n emits only new `place_id`s into the Clay table), and the baseline is appended after each run. Cold start: seed the baseline with one full initial snapshot and emit deltas only from the second month, so the first run does not flag the whole universe as new [INFERRED]. Secondary trigger: public opening announcements via Claygent web search over news, LinkedIn, Instagram, and Facebook (Nairametrics, Guardian, facility socials) [signal-matrix.md]. Facebook is reached through Google's index (`site:facebook.com` SERP), not by crawling Facebook directly, because Facebook has no public search API and blocks open crawling; candidate Pages are then enriched with an Apify Facebook Page scraper [INFERRED].
- **Verification + enrichment sources:** Google Places (facility categories, review count, location) [signal-matrix.md]; Claygent (website/company-domain check, specialty sub-classification, record-state discovery) [gtm-system.md]; HMO and LASHMA/ILERA EKO accredited-provider directories via Claygent web search (HMO participation) [company.md], [market.md]; PCN premises via Claygent web search (pharmacies) [market.md]; Apollo/Prospeo (headcount, firmographic, people) [gtm-system.md]; HubSpot/DokiTab customer and active-pipeline lookup table (suppression) [gtm-system.md] [INFERRED]. HEFAMAA registration is assumed (legally required in Lagos) and confirmed manually at the demo stage, not pulled in Clay [market.md], [signals.md].

## 4. Signals Table [ENGINE]

| # | Signal | Role | How it's read (Clay build) | Output |
|---|---|---|---|---|
| 1 | New Places listing (snapshot delta) | trigger | boolean: `place_id` first seen in the monthly Google Places snapshot <= 90 days, diffed vs a persisted baseline table | `is_new_listing` + `first_seen_date` |
| 2 | Public opening announcement | trigger (secondary) | boolean: Claygent web search finds a DATED opening post/article <= 90 days (news / LinkedIn / Instagram / Facebook; Facebook via Google index + Apify Page scrape, see note below). If no reliable date, treat as corroboration only and let Places `first_seen_date` carry recency | `is_new_opening` + `opening_ref` (+ `fb_page_url`, `fb_opening_ref`) |
| 3 | HEFAMAA registration | manual (not a Clay column) | Confirmed manually at the demo/sales stage; the portal is login-gated (per-facility QR only) and registration is legally required of every Lagos facility, so near-universal. Not triggered, gated, or scored in Clay | none in Clay |
| 4 | Facility vertical | router | Claygent AI enum from Places category + website: `multi_specialty_hospital` / `diagnostic_lab` / `pharmacy` / `specialty_clinic` / `teaching_or_ngo` [icp.md], [signal-matrix.md] | `segment` (S1 to S5) |
| 5 | Specialty sub-classification (S4) | router (S4 only) | Claygent AI enum from `facility_name` + Places categories + website/social bio: `Dental` / `Eye / Optometry` / `Physiotherapy` / `Maternity / Fertility` / `Diagnostics / Lab` / `Multi-Specialty` | `specialty` |
| 6 | Size band (headcount proxy) | router | Apollo employee-headcount band: `1-20` / `21-100` / `100+`; Google review count as a secondary volume signal (`<50` low, `>100` high); any bed mapping `[INFERRED]` [icp.md] | `size_band` |
| 7 | HMO panel participation | router | Claygent web-search boolean + source name; validate on 1 known positive + 1 known negative before scaling [company.md], [market.md] | `bills_hmo` + `hmo_name` |
| 8 | Company website present | router | formula/regex: facility has a corporate website domain (not gmail/yahoo/none) [gtm-system.md] | `has_company_website` |
| 9 | Verified work email present | router (post Step 13) | boolean set by the contact waterfall when a verified business email is found | `has_verified_work_email` (routes Email vs LinkedIn/WhatsApp) |
| 10 | PCN premises verification (S3) | router (S3 only) | Claygent web-search boolean; validate on 1 positive + 1 negative before scaling [market.md] | `pcn_verified` |
| 11 | Incumbent record state | context-clause (NEVER routes) | Claygent AI enum, discovery-confirmed not list-filterable: `paper` / `legacy_emr` / `open_source` / `unknown` [icp.md], [voice.md] | `record_state` (copy color only) |

**Count clusters (Signal 6, `size_band`)** [headcount proxy; bed mapping `[INFERRED]`, no bed data is reachable in Clay]:
- Apollo headcount `1-20` (or `<50` Google reviews) = small facility (owner-led, faster decision); `[INFERRED]` ~ up to 150 beds.
- Apollo headcount `21-100` = mid facility (Champion + Economic Buyer both engaged); `[INFERRED]` ~ 150 to 500 beds.
- Apollo headcount `100+` (or `>100` reviews) = large/teaching-adjacent (longer cycle, board-led); `[INFERRED]` ~ 500+ beds.

**Signal 2 Facebook sub-source (Google-indexed) `[INFERRED]`.** Facebook is a strong opening-announcement source in Nigeria (many clinics are Facebook-first), but it has no public search API and blocks open crawling, so it is not scraped directly. Pipeline: (1) scheduled Google SERP queries such as `site:facebook.com ("now open" OR "grand opening") (clinic OR hospital OR pharmacy OR diagnostic) {city}` return candidate Page/post URLs (`fb_page_url`); (2) an Apify Facebook Page scraper pulls founded date, recent posts, address, and phone from each public Page; (3) an AI column classifies opening language + recency (<= 90 days) to `is_fb_opening` + `fb_opening_ref`, which OR into `is_new_opening` / `opening_ref`. Caveats: public business Pages only, via Apify, never a logged-in personal account (ban / ToS risk); the Page "founded" date is not the opening date, so prefer a dated post and keep the Google Places `first_seen_date` as the harder trigger; match the Page to the facility on name + city + address before trusting it; run the 2-row validation gate (1 known-open, 1 not) before scaling.

## Qualification Rules (ICP fit gate) [ICP]

Applies the `gtm-system.md` Step 5 fit check [gtm-system.md]; thresholds `[INFERRED]` (no rubric exists in the files):
- IF `is_new_listing` OR `is_new_opening` is false THEN not triggered, drop.
- Suppression: IF the account matches the HubSpot/DokiTab customer or active-pipeline lookup (on `domain`, or normalized `facility_name + city`) THEN drop.
- IF `segment` in {S1, S2, S3, S4} THEN proceed; IF `teaching_or_ngo` (S5) THEN hold/deprioritize [icp.md].
- Score: +3 S1 Lagos multi-specialty/diagnostics, +2 Tier B (S2/S3/S4); +2 `is_new_listing`, +1 `is_new_opening`; +1 `bills_hmo`. Qualify at >= 4, hold 2 to 3, drop < 2 [INFERRED].
- Hard disqualify gates from Exclusion above override any score [icp.md].
- IF an S3 pharmacy fails PCN verification THEN hold that branch before paid enrichment [gtm-system.md].

---

## 5. Routing Matrix with Angles (the Angle Matrix) [ENGINE]

Routing axes (as named): `segment` (router) x persona/recipient, modulated by `bills_hmo` and `size_band`. Channel routing is separate from narrative: `has_company_website` is a company-level check, and `has_verified_work_email` (set after Step 13, the contact waterfall) routes Email vs LinkedIn/WhatsApp. Angles are meaning, not copy; `/campaign-copy` turns them into lines [workflow-anatomy.md].

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
| S3 x Champion (superintendent pharmacist) | `segment=S3` + trigger + `pcn_verified` | New pharmacy premises, PCN inspection ahead | Open PCN-ready with a controlled-drugs register and FEFO dispensing that keep running through outages [company.md], [market.md] |
| S3 x Economic Buyer (owner) | `segment=S3` + recipient owner | New retail/hospital pharmacy, thin margins | Free multi-till POS from day one; stop expiry write-offs and cashier leakage before they start [company.md], [voice.md] |
| S3 x Influencer (pharmacist/cashier) | `segment=S3` + recipient staff | Counter staff setting up dispensing/till | FEFO batch dispensing with expiry/reorder alerts and split-tender POS that reconciles each shift [company.md] |
| S4 x Champion (consultant) | `segment=S4` + trigger + `specialty` | New single-specialty clinic opening | 5-minute standalone mode tuned to the specialty (odontogram / Snellen-IOP / ROM-MMT / partograph), ward screens hidden, free to start [company.md] |
| S4 x Economic Buyer (owner) | `segment=S4` + recipient owner | Owner-operator funding the fit-out | Start free, no server, no per-seat fees; pay only if you later want AI scribe or telemedicine [company.md] |
| S4 x Influencer (nurse/front desk) | `segment=S4` + recipient staff | Front-of-house setting up charting | BYOD-safe charting from launch, biometric lock and offline sync, no paper folders [voice.md], [company.md] |

**Modulator routing (declared):**
- `bills_hmo`: IF true THEN lead the cell with HMO claim-rejection prevention (DokiTab's one proven outcome) [customers.md]; ELSE lead with compliance/greenfield records [icp.md]. New-HEFAMAA-only facilities are often pre-HMO, so they take the compliance lead; new branches of existing groups usually take the HMO lead [INFERRED].
- `size_band`: IF small (low Apollo headcount) or standalone THEN owner-led, low-friction free-workspace PLG CTA; IF mid or large THEN Champion + Economic Buyer dual thread, demo CTA [gtm-system.md], [personas.md] [INFERRED].

**Branch coverage:** no cell references a signal absent in it. IF `bills_hmo=false` no claim-rejection line (compliance instead). The dated reference comes from `first_seen_date` or `opening_ref` (see the `{{opening_ref}}` fallback in Section 6), never from HEFAMAA. `record_state` is context-clause color only and never the routing reason [icp.md].

## 6. Variables Contract [ENGINE]

| Variable | Source field | Exists in branches |
|---|---|---|
| `{{facility_name}}` | Google Places / baseline table | all |
| `{{facility_type}}` | Signal 4 enum (`segment`) | all |
| `{{city_area}}` | Google Places (city) | all |
| `{{neighborhood}}` | Google Places (sublocality); falls back to `{{city_area}}` when empty | all (used by the `{{opening_ref}}` fallback) |
| `{{first_seen_date}}` | Places snapshot delta (Signal 1) | new-listing branches |
| `{{opening_ref}}` | fallback hierarchy, see below | all (never blank) |
| `{{hmo_name}}` | HMO/ILERA EKO directory (Signal 7, Claygent) | `bills_hmo=true` only |
| `{{specialty}}` | Signal 5 specialty sub-classification | S4 only |
| `{{first_name}}` | Apollo / Prospeo | all contacted |
| `{{title}}` | Apollo / Prospeo | reserved (not used by current copy) |
| `{{email}}` | Step 13 contact waterfall (verified) | all contacted |

`{{opening_ref}}` fallback hierarchy (never renders blank): (1) a verified opening press/social snippet from Signal 2 (news, LinkedIn, Instagram, or the Facebook `fb_opening_ref`); (2) the phrase "a new listing in {{neighborhood}}" built from Google Places, where `{{neighborhood}}` itself falls back to `{{city_area}}` when the Places sublocality is empty. One of the two always resolves, because every row has either an opening announcement or a Places listing by construction.

Per-variable fallbacks (so copy never renders blank): `{{hmo_name}}` → "your HMOs"; `{{specialty}}` → "your clinic"; `{{first_seen_date}}` → omit (use `{{opening_ref}}` instead); `{{neighborhood}}` → `{{city_area}}`. Optional variables otherwise appear only in the branches where they resolve (per the copy waterfall).

Data-must-exist: any fact not delivered by enrichment cannot appear in copy. Specific product figures stay `{{placeholder}}` until `context/messaging/facts.md` exists [INFERRED].

## 7. Cadence & Channel [ENGINE]

- Periodicity: always-on. Run the Google Places snapshot + delta monthly and the opening-announcement search bi-weekly, matching signal velocity [workflow-anatomy.md], [signal-matrix.md] [INFERRED].
- Channel: email + LinkedIn for facilities with a verified work email (`has_verified_work_email=true`); WhatsApp/phone-first otherwise [gtm-system.md].
- Dedup: suppress 90 days after outreach; allow re-entry on a new signal [workflow-anatomy.md].

---

## 8. Clay Enrichment Steps [ENGINE]

Ordered waterfall build, cheap source first, paid only on survivors; prove each paid column on one qualifying and one rejecting row before scaling [build-clay], [gtm-system.md]. Credit counts `[INFERRED]`.

| Step | Clay build | Input to output |
|---|---|---|
| 1 | Source import: monthly Google Places snapshot of NG healthcare facilities, keyed by `place_id`, diffed against the persisted baseline (Google Sheet / n8n keyed by `place_id`; `is_new_listing = place_id NOT IN baseline`; seed the baseline on run 1, emit deltas from run 2), plus the opening-announcement search (news/LinkedIn/Instagram, and Facebook via Google SERP to Apify Page scrape to an AI opening classifier) | raw rows to `facility_name`, `place_id`, `city_area`, `neighborhood`, `first_seen_date`, `is_new_listing`, `is_new_opening`, `opening_ref`, `fb_page_url`, `fb_opening_ref` [signal-matrix.md] |
| 2 | Suppression dedup: match against the HubSpot/DokiTab customer + active-pipeline lookup on `domain` and normalized `facility_name + city` | drop existing customers/pipeline before any paid enrichment [gtm-system.md] |
| 3 | HEFAMAA (Signal 3): manual check at the demo/sales stage, not a Clay column | no Clay output; registration is assumed (legally required in Lagos) [market.md], [gtm-system.md] |
| 4 | Vertical classification (Signal 4): Claygent AI enum from Places category + website | to `segment` S1 to S5 [icp.md] |
| 5 | Specialty sub-classification (Signal 5, S4): Claygent AI enum from `facility_name` + Places categories + website/social bio | to `specialty` (Dental / Eye-Optometry / Physiotherapy / Maternity-Fertility / Diagnostics-Lab / Multi-Specialty) |
| 6 | Size band (Signal 6): Apollo employee-headcount band + Google review count (secondary) | to `size_band`; bed mapping `[INFERRED]` [icp.md] |
| 7 | HMO panel check (Signal 7): Claygent web search | to `bills_hmo` + `hmo_name`; validate on 1 known positive + 1 known negative before scaling [company.md], [market.md] |
| 8 | PCN premises check (Signal 10, S3): Claygent web search | to `pcn_verified`; validate on 1 positive + 1 negative before scaling [market.md] |
| 9 | Company website / domain (Signal 8): formula/regex on the facility website | to `has_company_website` [gtm-system.md] |
| 10 | Record-state discovery (Signal 11, context-clause): Claygent AI enum | to `record_state` (copy color only) [icp.md], [voice.md] |
| 11 | ICP fit score + gate: Clay formula (Qualification Rules) | signals to qualify / hold / drop [icp.md], [gtm-system.md] |
| 12 | People: Apollo/Prospeo + Clay Find People | to Champion / Economic Buyer / Influencer per segment [personas.md] |
| 13 | Contact waterfall: email find + verify, catch-all handling; sets `has_verified_work_email` (Signal 9); phone/WhatsApp fallback when no verified email | to `{{email}}` + channel + confidence; routes Email vs LinkedIn/WhatsApp [gtm-system.md] |

Gates mirror `gtm-system.md` Steps 4 to 8: suppression dedup runs before any paid column; cheap-source-resolves stops the waterfall; the HMO and PCN Claygent columns each return a confidence score (accept only above a set threshold), pass a 2-row validation (1 positive + 1 negative) before scaling, and get a periodic spot-check; an S3 pharmacy that fails PCN verification holds that branch; no verified work email routes to WhatsApp/phone; no email and no phone returns the account to sourcing [gtm-system.md].

## 9. Sequence Structure and Copy Handoff [ENGINE]

Copy is written and approved in `copy.md` (status COPY-APPROVED). The sequence STRUCTURE is below; `/campaign-copy` reads the Angle Matrix (Section 5) and writes the lines under its line laws.

- Channels: email + LinkedIn for `has_verified_work_email=true`; WhatsApp/phone-first for `has_verified_work_email=false` [gtm-system.md].
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

**Structure complete and COPY-APPROVED (2026-11-21).** Definition of done met: mother intent, inclusion/exclusion, sources, signals table (roles + clusters), angle matrix with every cell's angle, variables contract, cadence, enrichment steps, and reply-handling, all with Engine/ICP markers. Copy lives in `copy.md`; its e1l1 waterfall was re-pointed on 2026-11-21 to `is_new_opening` / `is_new_listing` to match this trigger architecture.
