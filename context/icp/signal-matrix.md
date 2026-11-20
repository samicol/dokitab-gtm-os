# DokiTab Signal Matrix

Work in progress, not a final deliverable. Validate with sales and leadership before building [signal-matrix-framework.md].

Built from `context/icp/icp.md`, `context/icp/personas.md`, `context/docs/research/*`, and the playbooks. Every claim is traced to a file. Judgement calls (scores, angles, data sources not named in the files) are marked `[INFERRED]`.

Scoring scale (neither playbook defines a per-test scale, so this is `[INFERRED]`): 1 = weak, 3 = moderate, 5 = strong, per test, total out of 20.

Segments (rows) from `context/icp/icp.md` tiers and facility types [icp.md]:
- S1: Lagos private multi-specialty hospitals and diagnostic centers that bill HMOs (Tier A beachhead)
- S2: Standalone diagnostic labs / PACS centers (Tier B)
- S3: Pharmacies, hospital and retail (Tier B)
- S4: Single-specialty clinics: dental, eye, physio, maternity (Tier B)
- S5: Teaching hospitals and donor/NGO public-health programs (Tier C, deprioritized)

Personas (columns) from `context/icp/personas.md` [personas.md]:
- Champion: CMD / consultant
- Economic Buyer: board / owner
- Influencer: pharmacist, lab scientist, billing/claims officer, records officer, IT/EMR admin, nurse, cashier

---

## Matrix 1: Segment x Persona Messaging Angle

Angles are in buyer language and tied to a research-backed pain. Angle wording is `[INFERRED]`; the pain each rests on is cited.

| Segment | Champion (CMD/consultant) | Economic Buyer (board/owner) | Influencer (dept leads) |
|---|---|---|---|
| S1 Multi-specialty hospital + diagnostics | "Cut HMO claim rejections and keep charting through NEPA cuts, on one record" [customers.md], [voice.md] | "Run the full hospital (wards, theatre, accounting) with no per-seat fees and no ₦12M+ server room" [voice.md], [company.md] | "Billing/claims officer: submit cleaner HMO claims; lab: panic-value alerts and HL7 feeds in the same record" [company.md] |
| S2 Standalone lab / PACS | "Digital results, barcode tracking and analyzer (Mindray/Sysmex/Roche) feeds without a server" [company.md] | "Free LIMS and PACS with no per-seat licensing; MLSCN ISO 15189 aligned" [company.md] | "Lab scientist: SMS panic-value alerts, home phlebotomy dispatch, results that don't live on paper" [company.md], [voice.md] |
| S3 Pharmacy | "PCN-ready controlled-drugs register and FEFO dispensing that survives outages" [company.md], [market.md] | "Free multi-till POS; stop expiry write-offs and cashier leakage" [company.md], [voice.md] | "Pharmacist: FEFO batch dispensing, expiry and reorder alerts; cashier: split-tender POS that reconciles" [company.md] |
| S4 Single-specialty clinic | "5-minute standalone mode for your specialty (FDI odontogram / Snellen-IOP / ROM-MMT / WHO partograph), ward screens hidden" [company.md] | "Start free, no server, no per-seat fees; upgrade only if you want AI scribe or telemedicine" [company.md] | "Front desk and nurse: BYOD-safe charting with biometric lock and offline sync" [voice.md], [company.md] |
| S5 Teaching hospital / NGO program | "Public-health registries (HIV/ART, TB DOTS, immunization) and an immutable audit trail" [company.md] | Deprioritized: long procurement cycle, no reference [icp.md], [customers.md] | "Records/M&E staff: structured registries over paper folders" [company.md], [voice.md] |

---

## Matrix 2: Buying Signals by Segment

Mother Intents are drawn from the catalog [workflow-anatomy.md]. Detection tools reference the Clay/Apify/Apollo actions in the playbooks [workflow-anatomy.md], [signal-matrix-framework.md] and the local `tools/` bundles (crawl4ai, rex). Four-test scores are `[INFERRED]`: (a) clear buying reason, (b) enough volume, (c) data you can get, (d) natural story.

| # | Signal | Primary segments | Mother Intent [workflow-anatomy.md] | Data Source | Detection Tool | a | b | c | d | Total |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | HEFAMAA registration / accreditation status, new registrations, pending inspection cohort | S1, S2, S3, S4 | Regulatory changes | HEFAMAA online registry with public QR verification, live since Dec 2025 [market.md], [signals.md] | crawl4ai (`tools/crawl4ai`) or Clay HTTP scrape [signal-matrix-framework.md] `[INFERRED]` | 4 | 4 | 4 | 5 | 17 |
| 2 | HMO panel participation and new HMO/ILERA EKO onboarding | S1, S2, S4 | Regulatory changes / intent | HMO provider directories (Hygeia, AXA, NHIA) and LASHMA/ILERA EKO accredited-provider lists [company.md], [market.md] | crawl4ai or Clay HTTP scrape `[INFERRED]` | 5 | 4 | 3 | 4 | 16 |
| 3 | Hiring records officer, billing/claims officer, IT/EMR admin, or lab scientist | S1, S2, S5 | Hiring specific roles | LinkedIn Jobs, Jobberman, Indeed NG [signals.md] | Clay "Find Jobs at Company" / Apify LinkedIn Jobs / Apollo [workflow-anatomy.md] | 4 | 3 | 4 | 4 | 15 |
| 4 | New branch or new lab/pharmacy opening | S1, S2, S3 | New product launches / expansion | Press (Nairametrics, Guardian), Google Business/Maps, facility socials [signals.md] | Google Places API via Clay HTTP; news scrape via crawl4ai `[INFERRED]` | 5 | 2 | 3 | 5 | 15 |
| 5 | PCN premises licensing, new pharmacy registration, or recent sealing | S3 | Regulatory changes | PCN "From the Registrar's Desk" newsletters (pcn.gov.ng), press on sealings [market.md], [signals.md] | crawl4ai / Clay HTTP scrape `[INFERRED]` | 4 | 3 | 3 | 4 | 14 |
| 6 | New CMD / Medical Director (first 90 days) | S1, S5 | New leadership | LinkedIn / Apollo [workflow-anatomy.md] | Clay "Find People" + job-change filter / Apollo [workflow-anatomy.md] | 4 | 2 | 3 | 4 | 13 |
| 7 | Facility density by area (list/TAM build, compound only) | S1, S2, S3, S4 | Lookalike / physical-world | Google Places API [signal-matrix-framework.md] | Clay HTTP; pair with signals 1-3, never alone [signal-matrix-framework.md] | 2 | 5 | 4 | 2 | 13 |

Notes:
- Signals 1, 3, 4, 6, 7 are **universal** (same build, swap keywords/filters per segment) [signal-matrix-framework.md]. Signals 2 and 5 are **niche-specific** (unique Nigerian regulatory/HMO data sources) [signal-matrix-framework.md].
- Strongest research-backed pain (HMO claim rejections) is discovery-only at the account level; signal 2 catches the *panel-participation* proxy, not the rejection rate itself [icp.md], [customers.md].
- Signal 7 is a list builder, not a timing trigger; it has no buying story on its own [signal-matrix-framework.md].

---

## Top Signals to Build First

Ranked by four-test total and fit to DokiTab's positioning (free, offline-first, HMO claims, compliance) and the Lagos beachhead [icp.md], [voice.md].

1. **HEFAMAA registration / accreditation (17).** Public registry with QR verification (Dec 2025) makes it the rare signal that is high-volume AND externally findable AND Lagos-concentrated, matching the beachhead [market.md], [icp.md]. Passes all four tests; the compliance story is natural and local. Build first as both a new-registration feed and an inspection-cohort list.
2. **HMO panel participation / new onboarding (16).** Maps to DokiTab's one proven outcome, cutting HMO claim rejections [customers.md]. Directories give volume; the claims angle is the sharpest pain in the research. Use as the primary list and as a routing layer on top of signal 1.
3. **Hiring records/billing/EMR/lab roles (15).** Universal, build-once signal with the cleanest data path (Apify/Clay) and clear digitization intent [signals.md], [workflow-anatomy.md]. Replicates across S1, S2, S5 with keyword swaps.
4. **New branch opening (15, low volume).** Highest buying intent and best story (acute multi-branch need), but volume and data are weak, so build it as a lighter, lower-cadence play paired with signal 1 rather than a standalone workflow [signals.md], [signal-matrix-framework.md].
5. **PCN premises / new pharmacy registration (14).** The pharmacy-segment (S3) equivalent of signal 1; build when S3 becomes a focus, using PCN newsletters and sealing reports [market.md], [signals.md].

Deprioritized now: new-leadership (6) and facility-density (7), both scoring 13, on thin Nigerian data coverage and (for 7) no standalone story [signal-matrix-framework.md] `[INFERRED]`.

## Open gaps (block full Iron Law sign-off)
- No scrape yet proven against the HEFAMAA registry, HMO directories, or PCN PDFs; findability is asserted from the research, not tested [icp.md], [signals.md].
- No volume counts pulled for any Nigerian source; (b) scores are estimates `[INFERRED]`.
- Account-level HMO claim-rejection rate (the core pain) still has no external proxy [icp.md].
- No Matrix 3 workflow tracker generated yet; produce it once priority signals are confirmed with stakeholders [signal-matrix-framework.md].
