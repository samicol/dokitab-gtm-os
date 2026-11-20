# DokiTab ICP

Built only from facts verified in `context/docs/research/` (checked 2026-11-20). Every row cites its source file. Where research does not support a criterion, it is in Open Questions, not invented.

## Primary beachhead (call these first)
Lagos-based private multi-specialty hospitals and diagnostic centers (roughly 50 to 500+ beds) that bill HMOs. This is the only profile research actually backs with a reference: the one named outcome comes from a "Multi-Specialty Hospital & Diagnostic Center, Ikeja, Lagos" with HMO claim-rejection pain [customers.md], DokiTab keeps data in-country on AWS Lagos [company.md], and Lagos is where the strongest regulatory triggers concentrate (HEFAMAA, LASHMA/Ilera Eko) [market.md]. Everything below widens from this core; treat non-Lagos and non-hospital segments as expansion, not proven fit. [INFERRED FROM RESEARCH]

Segment priority for prospecting:
- Tier A (now): Lagos private multi-specialty hospitals and diagnostic centers that bill HMOs [customers.md], [company.md], [market.md].
- Tier B (test): standalone diagnostic labs/PACS centers, hospital and retail pharmacies, and single-specialty clinics (dental, eye, physio, maternity) that DokiTab pitches standalone plug-and-play mode to [company.md], [customers.md].
- Tier C (deprioritize): teaching hospitals and donor/NGO public-health programs. Named as a target market [company.md] but long cycles, no reference, and no evidence of fit [customers.md].

## Layer 1: Firmographic

| Attribute | Criteria | Source File |
|---|---|---|
| Facility types | Tier A: multi-specialty hospitals and diagnostic centers. Tier B: standalone labs/PACS, pharmacies, single-specialty clinics. Tier C: teaching hospitals, donor/NGO programs | [customers.md], [company.md] |
| Bed size | 50 to 500+ beds (hospitals); bed size not applicable to standalone lab/pharmacy/clinic targets | [customers.md], [company.md] |
| Ownership | Private facilities (primary); donor/NGO public-health programs (Tier C only) | [company.md] |
| Geography | Primary: Lagos (only location with a named reference, in-country data, and densest regulatory triggers). Secondary: rest of Nigeria. Unproven: wider West Africa (stated market, no deployment evidence) | [customers.md], [company.md], [market.md] |
| Specialties (standalone mode) | Dental, eye/optometry, physiotherapy, maternity/ANC, pharmacy, diagnostics (Tier B). Oncology/dialysis are modules, no target evidence | [company.md] |

Staff-count and patient-volume thresholds: not set. The free tier removes per-seat cost, so value rises with headcount [company.md], but no numeric cut-off is verified. See Open Questions.

## Layer 2: Technographic

Note: this layer is DokiTab's framing of market pain [voice.md], not independently verified facility data. These are discovery-call qualifiers, not list-filter attributes, unless a column says otherwise.

| Attribute | Criteria | Source File |
|---|---|---|
| Current record state | Paper folders ("losing records to paper folders") or legacy tiered EMRs that lock wards/theatre/accounting behind higher tiers. Confirm on discovery; not externally filterable | [voice.md], [competitors.md] |
| Incumbent software | Per-seat legacy EMRs at "₦25,000 to ₦50,000 per user seat", or self-hosted open-source (OpenMRS, Bahmni, OpenEMR). Named rivals a rep may hear: Helium Health, eClinicPlus, Hyella | [voice.md], [competitors.md] |
| Server/hardware | Avoiding "₦12M–₦22M" server rooms; runs with no static IP (Cloudflare Tunnel) | [voice.md], [company.md] |
| Connectivity/power | Unreliable grid ("NEPA" blackouts) and intermittent internet; needs offline-first SQLite sync | [voice.md], [company.md], [market.md] |

## Layer 3: Intent Signals

Added a findability column: a signal a rep cannot see on a prospect is a discovery-call topic, not a list-building filter.

| Signal | Criteria | Findable externally? | Source File |
|---|---|---|---|
| HMO claim rejections | Billing HMOs/state schemes and losing revenue to rejections | Partly: HMO participation sometimes shown on facility sites/HMO directories; rejection rate is discovery-only | [signals.md], [market.md], [customers.md] |
| Regulatory registration/inspection | HEFAMAA/PCN registration status, licensing renewal, or recent sealing | Yes: HEFAMAA moved to an online registry with public QR verification (Dec 2025); PCN sealings reported in press | [signals.md], [market.md] |
| Expansion | New branch, or adding a lab/pharmacy | Yes, no feed yet: press, social, Google Business listings | [signals.md] |
| Hiring | Posting records officer, billing/claims officer, IT/EMR admin, or lab scientist roles | Yes, no feed yet: job boards, LinkedIn. No data pulled, and signals.md marks this inferred | [signals.md] |
| Data-protection exposure | Secondary/tertiary hospital subject to NDPA controller registration | No external feed; inferred from facility tier | [signals.md], [market.md] |
| Operational pain | Outage-driven downtime/data loss, or cashier/revenue leakage | No: discovery-only | [signals.md], [voice.md] |

## Layer 4: Persona

| Attribute | Criteria | Source File |
|---|---|---|
| Primary decision-maker | Chief Medical Director (CMD) | [customers.md], [company.md] |
| Champion | CMD / consultant | [customers.md], [company.md] |
| Economic buyer | Facility board (named "board-ready reports" audience); owner/proprietor [INFERRED FROM RESEARCH] | [company.md] |
| Influencers | Pharmacist, lab scientist, billing/claims officer, health records officer, IT/EMR administrator, nurse, cashier | [signals.md], [market.md], [company.md], [voice.md] |

## Layer 5: Engagement Readiness (acute, dated triggers)

Readiness = the same conditions as Intent but with a clock on them. Prioritize an account when a trigger below has a date.

| Trigger (has a deadline/event) | Criteria | Source File |
|---|---|---|
| HMO rejection bleed this quarter | Measurable claim-revenue loss happening now | [customers.md], [signals.md] |
| Dated compliance event | Scheduled HEFAMAA/PCN inspection, licensing renewal, or a recent sealing to remediate | [signals.md], [market.md] |
| Branch opening in flight | New site or new lab/pharmacy opening on a known date | [signals.md] |
| Just lost data to an outage | Recent downtime/data loss on a cloud-only system | [voice.md], [signals.md] |
| At a legacy renewal/price-hike decision | Incumbent per-seat or server contract up for renewal | [voice.md], [company.md] |

## Disqualification / Not a Fit

- **Outside Nigeria (and unproven in wider West Africa).** Stated market is Nigeria and West Africa, but only Lagos has evidence; treat non-Nigeria as out of profile for now [company.md], [customers.md].
- **Need a fully air-gapped system with no server sync.** The offline queue replays to a server over Cloudflare Tunnel and stores data on AWS Lagos; zero external data egress does not fit [company.md] [INFERRED FROM RESEARCH].
- **Need AI scribe, telemedicine, or multi-branch governance at zero cost.** Those are on the paid ₦30,000/month plan, not the free tier [company.md].
- **Require an independently verified vendor track record before buying.** No named customers, case studies, funding, or reviews are public [customers.md].
- **Require formal HIPAA (or equivalent) certification.** DokiTab says "Designed to satisfy HIPAA (US) security rules" (FAQ: "meets HIPAA security standards") but no certification evidence exists [voice.md], [company.md].

## Open Questions / Research Gaps (ICP criteria not yet supported)

- No verified staff-count or patient-volume threshold to filter target lists; only hospital bed range (50 to 500+) is sourced [company.md].
- No way yet to detect HMO claim-rejection pain externally (the strongest buying signal is discovery-only); a findable proxy is needed [signals.md], [customers.md].
- BYOD prevalence at target facilities is unknown. "BYOD Nurse Workflows" is a DokiTab content theme, not a verified prospect attribute, so it was removed as a targeting criterion [voice.md].
- No evidence of any deployment or traction outside Lagos; rest-of-Nigeria and West Africa targeting is unproven [customers.md], [company.md].
- No facility-level data feeds built yet for the externally-findable signals (HEFAMAA registry, job boards, branch-opening news) [signals.md].
- Competitor install base is unknown, so no rip-and-replace/displacement targeting is possible [competitors.md].
- No revenue, budget, or ability-to-pay threshold (no competitor or DokiTab pricing-power data) [market.md], [competitors.md].
- Ownership mix only partially supported (private per directories; NGO/donor per homepage); public/government and faith-based facilities unconfirmed [company.md].
- Decision-maker titles beyond CMD, consultant, and board are unconfirmed (owner/MD, procurement, administrator) [company.md], [signals.md].
- No named customers to pattern-match the ICP against [customers.md].
