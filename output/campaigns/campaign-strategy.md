# DokiTab Outbound Campaign Strategy

Generated with the `campaign-strategy` skill. Grounded in `CLAUDE.md`, `context/`, and `knowledge-base/playbooks/workflow-anatomy.md`. Estimates marked `[INFERRED]`. No em dashes.

**Core value proposition:** run the whole facility on one offline-first record for free, and keep charting and HMO claims working through every power cut [messaging.md], [company.md].
**Key differentiators:** free core tier, no per-seat fees, no server CapEx, offline-first through outages, in-country data on AWS Lagos [positioning.md], [company.md].
**Base ICP:** Lagos private multi-specialty hospitals and diagnostic centers (beachhead), plus standalone labs, pharmacies, and single-specialty clinics [icp.md].

## Customer Discovery Analysis

- Only ONE customer story exists and it is anonymous: a CMD of a "Multi-Specialty Hospital & Diagnostic Center, Ikeja, Lagos" who cut quarterly HMO claim rejections from 18% to under 1.5% [customers.md]. No named customers, logos, case studies, funding, or reviews exist [customers.md].
- Targeting read: the one proof point IS the beachhead (Lagos multi-specialty hospital + diagnostics billing HMOs), so Lookalike and the top signal-led plays target that profile [icp.md], [customers.md].
- Copy constraint: no named customer may appear in copy until `context/messaging/facts.md` authorizes it; Lookalike uses the profile, not a name [customers.md].

## Ranked Campaign Ideas (by Signal Strength + Data Ease)

| # / Name | Type | Mother Intent | Segment & Persona | Signal & Data Source | Outreach Angle | Vol/mo | Difficulty + Rationale (signal / data) |
|---|---|---|---|---|---|---|---|
| 1 HEFAMAA New Registration | Signal-Led | Regulatory changes [workflow-anatomy.md] | S1-S4; Champion CMD + EB owner [icp.md], [personas.md] | New HEFAMAA registry entry <=90d; public registry + QR, crawl4ai/Clay HTTP [signal-matrix.md], [market.md] | Greenfield: start compliant and free from day one, offline-first [messaging.md], [positioning.md] | 50-150 [INFERRED] | Medium; signal high (public compliance trigger), data high (public registry) [signal-matrix.md] |
| 2 HMO Panel / New Onboarding | Signal-Led | Regulatory changes / intent | S1; Champion CMD + Influencer billing officer [personas.md] | On HMO/ILERA EKO provider directory; scrape directories [signal-matrix.md], [company.md] | Cut HMO claim rejections (18% to under 1.5% proof) [customers.md], [messaging.md] | 100s list, dozens new [INFERRED] | Medium; signal strong (revenue pain), data medium (onboarding timing hard) [icp.md], [signal-matrix.md] |
| 3 Digitization Hiring | New Hire / Signal-Led | Hiring specific roles [workflow-anatomy.md] | S1,S2,S5; Influencer IT/EMR admin + Champion [personas.md] | Job posts for records/billing/EMR/lab roles; LinkedIn/Jobberman/Indeed via Clay Find Jobs/Apify [signal-matrix.md] | They are staffing to digitize; DokiTab does it free, no per-seat [messaging.md] | 20-60 [INFERRED] | Low; signal good, data easy (job scrape) [signal-matrix.md] |
| 4 New CMD / Medical Director | New Hire | New leadership past 90 days [workflow-anatomy.md] | S1,S5; Champion CMD [personas.md] | New CMD start <90d; LinkedIn/Apollo job-change [workflow-anatomy.md] | New-leader quick win: clean records + HMO claims in first 90 days [messaging.md] | 10-30 [INFERRED] | Medium; signal good, data thin for NG CMDs [signal-matrix.md] |
| 5 Lookalike (Ikeja profile) | Lookalike | Lookalike of customers [workflow-anatomy.md] | S1; Champion CMD + EB [icp.md], [personas.md] | Facilities matching the proof profile (Lagos multi-specialty + diagnostics, HMO-billing); match via HEFAMAA + Places + HMO directory [customers.md], [signal-matrix.md] | "Facilities like yours cut HMO rejections to under 1.5%" (profile, no name) [customers.md], [messaging.md] | 50-150 [INFERRED] | Medium; signal relevant, data medium; only one anonymous proof [customers.md] |
| 6 Standalone Lab LIMS | Signal-Led / Vertical | Niche-specific play | S2; Champion lab owner + Influencer lab scientist [personas.md] | Diagnostic labs via Google Places + HEFAMAA [signal-matrix.md] | Free LIMS + PACS, HL7 feeds (Mindray/Sysmex/Roche), no server [company.md], [positioning.md] | 40-100 [INFERRED] | Medium; signal moderate, data easy (Places) |
| 7 Pharmacy PCN / FEFO | Signal-Led / Vertical | Regulatory changes | S3; Champion superintendent pharmacist + Influencer cashier [personas.md] | Pharmacies via PCN premises list + Places [market.md], [signal-matrix.md] | PCN-ready controlled-drugs register + FEFO, free multi-till POS, stop expiry write-offs [company.md], [messaging.md] | 50-150 [INFERRED] | Medium; signal moderate, data medium (PCN PDFs) |
| 8 New Branch Opening | Signal-Led | New product launches / expansion [workflow-anatomy.md] | S1,S2,S3; EB owner + Champion [personas.md] | New branch via Places new-listing delta + press, crawl4ai [signal-matrix.md] | Multi-branch governance; run the new site free from day one [company.md], [messaging.md] | <20 [INFERRED] | High; signal strong, data low (volume + scrape) [signal-matrix.md] |
| 9 PCN New Pharmacy Premises | Signal-Led | Recently Incorporated / Regulatory | S3; Champion pharmacist [personas.md] | New PCN premises registrations via PCN newsletters [market.md] | Open PCN-ready with FEFO from day one [company.md] | 20-50 [INFERRED] | Medium-High; signal good, data messy (PDFs) |
| 10 Single-Specialty Clinic Standalone | Vertical / Signal-Led | Niche-specific play | S4; Champion consultant + Influencer nurse [personas.md] | Dental/eye/physio/maternity clinics via Places + HEFAMAA [signal-matrix.md], [icp.md] | 5-minute standalone mode tuned to the specialty, free [company.md], [messaging.md] | 60-150 [INFERRED] | Low-Medium; signal moderate, data easy |
| 11 Sanctioned / Sealed Remediation | Signal-Led (risk) | Regulatory changes | S1-S4; Champion CMD + EB [personas.md] | Recently sealed/sanctioned facilities via HEFAMAA/PCN enforcement press [signals.md], [market.md] | Get compliant fast: records, immutable audit, NDPA alignment [company.md], [messaging.md] | <10 [INFERRED] | High; signal strong but sensitive, data low and episodic; tone care [INFERRED] |
| 12 Newly Incorporated Private Hospital | Signal-Led | Recently Incorporated Businesses [workflow-anatomy.md] | S1,S4; EB owner + Champion [personas.md] | New facility via HEFAMAA new-reg [market.md], [signal-matrix.md] | Greenfield free setup, no server CapEx [messaging.md] | 30-80 [INFERRED] | Medium; overlaps #1, data public |
| 13 Creative Ideas (ICP) | Creative Ideas | Niche-specific play [workflow-anatomy.md] | S1-S4; Champion CMD [personas.md] | Base ICP list; AI reads facility website to generate 3 DokiTab use-cases | "I had an idea for {{facility_name}}": 3 specific ways to cut rejections, expiry, downtime [messaging.md] | base list [INFERRED] | Low; no signal (weak urgency), data easy (website) |
| 14 Multi-Branch Group, No Unified EMR | Niche / Stretch | Have/don't have a system | S1; EB board + Champion [personas.md] | Groups with 2+ branches via Places count; infer no central EMR [signal-matrix.md] [INFERRED] | One record across branches, multi-branch governance [company.md] | <20 [INFERRED] | High; signal good, data low (absence hard to prove) [INFERRED] |
| 15 Facility Density Broad List | Broad / No-AI | Mass sending campaigns | All S1-S4; mixed [personas.md] | Google Places full facility list by LGA [signal-matrix.md] | Free offline-first OS, no per-seat (static value prop) [messaging.md] | 100s [INFERRED] | Low build; signal weak (no trigger), data easy (Places) |
| 16 Creative (non-ICP): Partners & Referrers | Creative / Non-ICP | Niche-specific play | NON-ICP: analyzer/equipment distributors, HMO/ILERA EKO panel managers, clinic-setup consultants [INFERRED adjacency] | Scrape distributor/consultant sites + HMO admin contacts [INFERRED] | Refer or resell: your clients need free records that cut HMO rejections [messaging.md], [positioning.md] | <30 [INFERRED] | Medium; signal indirect, data medium; untouched audience |
| 17 NGO / Donor Public-Health Programs | Vertical (Tier 3) | Niche-specific play | S5; M&E/records staff + program lead [icp.md], [personas.md] | Donor/NGO health programs via public program directories [company.md] [INFERRED] | Free registries (HIV/ART, TB DOTS, immunization), immutable audit [company.md] | <15 [INFERRED] | High; long cycle, data low; deprioritized [icp.md] |

## Required Campaign Types (checklist)

- Creative Ideas (ICP): #13 [workflow-anatomy.md].
- New Hire: #3 (role hiring) and #4 (new leadership) [workflow-anatomy.md].
- Lookalike: #5 (profile-based, no named customer) [customers.md].
- Creative (non-ICP): #16 (partners and referrers outside the ICP).
- Tier 1 (build first): #1, #2, #3 all strong on signal and data [signal-matrix.md].

## No-AI Campaign

- **#15 Facility Density Broad List.** Works without AI: a tight, static value prop ("100% Free, offline-first, no per-seat fees") to a clean Google Places facility list by LGA [messaging.md], [signal-matrix.md]. Message structure: one-line free-forever hook, one proof line (offline through NEPA), one CTA to start a free workspace. No personalization needed because the offer itself is the hook [INFERRED].

## Front-End Offer Suggestions

1. **Free workspace (PLG self-serve):** start the free tier and see value before any call; fastest path to activation [company.md], [gtm-system.md].
2. **3-Year TCO calculator:** DokiTab already surfaces a "Hospital TCO Calculator" and a 3-year TCO guide; use it as a cold front-end to quantify per-seat and server savings [company.md], [voice.md].
3. **HMO claim-rejection audit:** offer a quick review framed on the 18% to under 1.5% proof, since rejections are the sharpest pain [customers.md].
4. **NDPA / PCN compliance checklist:** a value-add tied to the registration and inspection triggers [market.md], [signals.md].

## Next Step

Tier 1 and already specced: #1 (New Facility / HEFAMAA) is built to STRUCTURE-APPROVED at `output/campaigns/facility-expansion/workflow-spec.md`. Build the next with `/build-workflow-spec` (recommended: #2 HMO Panel or #3 Digitization Hiring), then `/campaign-copy`. Do not build all at once: build one, launch, learn, pick the next.
