# DokiTab: Company and Product

Raw research, not conclusions. All facts checked 2026-11-20. First-party = dokitab.com. Third-party = directories, press, other sites.

## Sourcing note
- The homepage and knowledge base were read via an automated fetch of `https://dokitab.com/` and `https://dokitab.com/blog` (canonical `https://dokitab.com/resources`). `/pricing`, `/about`, `/features`, and `/contact` all returned HTTP 404 on 2026-11-20, so there is no dedicated first-party pricing or about page at those paths.
- Long passages on the homepage were paraphrased during fetch; only short phrases are quoted verbatim. For fuller verbatim wording, `https://dokitab.com/index.md` (the markdown source) and `https://dokitab.com/resources.md` can be fetched directly. [INFERRED] these markdown sources exist based on the homepage linking its own `.md`.

## What they sell (first-party, dokitab.com homepage)
- Positioned as an offline-first healthcare operating system, hospital management system (HMS), and EMR.
- Homepage headline, verbatim: "The Operating System for African Healthcare, EMR & Diagnostics."
- Page title, verbatim: "DokiTab: Healthcare OS, Hospital EMR, Pharmacy & Lab LIMS."
- Self-claim, verbatim: "#1" offline-first Healthcare OS, HMS and EMR in Nigeria, and "#1 Across All Boards." No third-party source is cited for the ranking claim.
- Module count is stated inconsistently on the page: "69" modules in one place and "60+" core modules in another.

## Core and specialty modules (first-party, homepage; descriptions paraphrased unless quoted)
- EMR: patient histories, prescriptions, lab results.
- Pharmacy: FEFO batch dispensing, expiry tracking, reorder alerts, cashier shifts, multi-till POS, controlled-drugs register (references PCN 2026).
- Diagnostics / Lab (LIMS): HL7 analyzer feeds (names Mindray, Sysmex, Roche), SMS panic-value alerts, digital results, home phlebotomy dispatch.
- Radiology: PACS/DICOM viewer (X-ray, CT, MRI, ultrasound).
- Blood bank: ABO/Rh crossmatch, cold-chain logs, ISBT 128.
- Wards, theatre (WHO surgical checklist), emergency triage (4-tier ESI), digital MAR, SBAR handovers.
- Specialty suites: dental (FDI odontograms), eye/optometry (Snellen, refraction, IOP), physiotherapy (ROM, MMT, VAS), maternity/ANC (WHO partograph, APGAR), oncology (BSA dosing), dialysis, public-health registries (HIV/ART, TB DOTS, malaria, NPI immunization).
- Billing: HMO claims (names NHIA, LASHMA, Hygeia, AXA, "60+ HMO"), co-pay split billing, multi-till cashier with split tender (POS, NIBSS transfer, cash, Paystack/Flutterwave).
- Accounting: double-entry GL, PAYE schedules, debtors.
- Operations: HR roster, CSSD, assets, mortuary, staff and schedule management, GPS geofencing, analytics/board reports.
- AI Clinical Intelligence (paid 30k plan): voice-to-SOAP scribe, decision support, drug-interaction checks, OCR digitization of paper folders.
- Telemedicine (paid 30k plan): HD video consultations, virtual waiting room, multi-branch governance.

## Pricing model (first-party, homepage)
- Free Edition: ₦0/month, verbatim "100% Free (₦0/month, zero hidden charges)", "no expiry and no per-seat fees", unlimited staff accounts. Covers hospital, clinic, pharmacy, lab, dental, eye, physio operations, including wards, theatre, PACS, blood bank, GL/PAYE, HMO claims, and offline SQLite sync.
- 30k Plan: ₦30,000/month, described as "the only paid plan". FAQ also states ₦300,000/year. Adds AI Clinical Intelligence, telemedicine, multi-branch governance, and "Priority 24/7 Clinical Engineering" with WhatsApp support.
- Add-ons: prepaid SMS packs at ₦5 per SMS, WhatsApp packs at ₦12, and a 0.5% digital payment gateway settlement fee.
- Referral: ₦15,000 billing credit plus 1,000 free Termii SMS credits after a new facility logs its first 5 clinical encounters or pays an invoice.

### Third-party corroboration and conflict on pricing
- GoodFirms corroborates the free edition and the optional ₦30,000/month plan adding AI clinical decision support and telemedicine. Source: https://www.goodfirms.co/software/dokitab (checked 2026-11-20).
- SaaSworthy contradicts it: a FAQ states DokiTab does not offer a free plan and lists the free trial as "Not available." Source: https://www.saasworthy.com/product/dokitab/pricing (checked 2026-11-20).
- GetApp shows "No pricing info." Source: https://www.getapp.com/all-software/a/dokitab/ (checked 2026-11-20).
- [INFERRED] The first-party homepage is the strongest evidence; the SaaSworthy "no free plan" line is likely stale directory data, but this is a deduction, not confirmed.

## Target markets / verticals
- First-party (homepage): Nigeria and West Africa; 50 to 500+ bed multi-branch and teaching hospitals, specialty and outpatient clinics, standalone labs and PACS centers, retail and hospital pharmacies, donor/NGO public-health programs.
- Third-party (GetApp): "private clinics, hospitals, diagnostic centers, community pharmacies, and specialty practices across Nigeria and West Africa." Source: https://www.getapp.com/all-software/a/dokitab/ (checked 2026-11-20).

## Tech / security positioning (first-party, homepage)
- Compliance claims: verbatim "Designed to satisfy HIPAA (US) security rules"; FAQ states more strongly "dokitab meets HIPAA security standards". Also names NDPA 2023, POPIA (South Africa), Kenya DPA 2019, Ghana DPA 2012, NHIA Act 2022, PCN 2026 S.I. No. 8, MLSCN ISO 15189, and RRBN.
- Data residency: data said to stay in-country on AWS Lagos Local Zone.
- Encryption: TLS 1.3 via Cloudflare; offline queue is AES-256 encrypted SQLite.
- Access controls: GPS geofencing locks ward devices outside the hospital; CMD/consultant 24/7 remote access; biometric lock after 2 minutes of inactivity; screenshots blocked at OS level.
- Audit: immutable, append-only, server-side audit log.
- Network: Cloudflare Tunnel, no static IP needed.
- Offline behavior: during power cuts or internet drops, vitals, fluid balance, MAR doses, and SOAP notes are held locally and replayed when connectivity returns.
- Third-party (GetApp) echoes "encrypted tunnels, backups, and biometric locks" and alignment with HIPAA, NDPA, POPIA, PCN 2026. Source: https://www.getapp.com/all-software/a/dokitab/ (checked 2026-11-20).

## Funding
- Not found. No funding figure appears on dokitab.com or in any directory/press result checked 2026-11-20.

## Team size and leadership
- Team size: Not found.
- Leadership / founders: Not found. No names on the homepage; no DokiTab LinkedIn company or team page surfaced in search on 2026-11-20.
- Vendor legal name (third-party, GetApp AU): "DokiTab Healthcare Technologies." Source: https://www.getapp.com.au/software/2393899/dokitab (checked 2026-11-20).
- Founded year (third-party, GoodFirms): 2024; HQ Nigeria. Source: https://www.goodfirms.co/software/dokitab (checked 2026-11-20). Not confirmed first-party.

## Other first-party assets found
- Privacy policy: https://dokitab.com/privacy.html
- Knowledge base: https://dokitab.com/resources (also served at /blog)
- MCP endpoint page: https://dokitab.com/mcp
- YouTube: https://www.youtube.com/@dokitab
- Homepage also links "Hospital TCO Calculator", "2026 Best EMR Ranking", and "Compare Plans" (target URLs not yet read).
