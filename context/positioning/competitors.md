# DokiTab Competitive Positioning

Grounded only in `context/docs/research/competitors.md` and related research. No em dashes.

## Comparison Table

| Competitor | What it is | How it stands vs DokiTab | Source |
|---|---|---|---|
| Helium Health | Well-funded, multi-country Nigerian EMR/HMIS incumbent (Nigeria, Ghana, Liberia, Senegal, plus Saudi Arabia from 2021); offline-capable; sells loans, HeliumPay, telemedicine on top | Established and funded, with a track record DokiTab lacks; DokiTab attacks on free pricing and offline resilience | [competitors.md] |
| OpenMRS / Bahmni / OpenEMR | Open-source EMR and hospital systems; OpenMRS used across Africa; Bahmni built on OpenMRS | Free to license but must be self-hosted and maintained; DokiTab is free AND fully hosted with offline sync | [competitors.md] |
| eClinicPlus, Hyella | Named by DokiTab as legacy competitors; not independently researched | Treated as per-seat legacy EMRs in DokiTab's own framing; details unverified | [competitors.md] |
| Other named rivals (Odoo, Punch Me 360, Kena, Prokip, VirtualRx) | Named by DokiTab on its site; not independently researched | Unverified; confirm before using in sales | [competitors.md] |
| Legacy per-seat EMRs (generic) | Priced "₦25,000 to ₦50,000 per user seat" and need ₦12M to ₦22M server rooms | DokiTab removes per-seat fees and server CapEx entirely | [voice.md], [competitors.md] |

Note: "Insta Health" and "Hydra / AfriHealth" are not present in the research. `competitors.md` lists Hydra/AfriHealth (and OneCare, MocDoc) as "not found / not verified," so do not cite them as confirmed competitors [competitors.md].

## Where DokiTab Wins

- Price: a free core tier, no per-seat fees, and no server-room CapEx, against legacy per-seat and server costs [voice.md], [company.md].
- Offline-first: charting, MAR, and claims keep running through power and internet outages [company.md], [voice.md].
- Breadth in the free tier: wards, theatre, pharmacy, lab, PACS, blood bank, accounting, and HMO claims, which legacy rivals lock behind higher tiers [voice.md], [company.md].
- Local fit: naira pricing, Nigerian HMOs (NHIA, LASHMA, Hygeia, AXA), and "NEPA"-outage design [company.md], [voice.md].

## Where DokiTab Loses / Trade-Offs

- No public track record: no named customers, case studies, funding, or reviews [customers.md].
- Compliance claims not independently verified; the site says "Designed to satisfy HIPAA (US) security rules" on the homepage but "meets HIPAA security standards" in the FAQ, a claim to use carefully [voice.md].
- Helium Health is better funded, multi-country, and established [competitors.md].
- Not air-gapped: the offline queue replays to a server on AWS Lagos, so a buyer requiring zero external data egress is not a fit [company.md].

## How to Respond in Sales Conversations

- Against legacy per-seat EMRs: lead with the free tier and no server CapEx, and offer a free parallel pilot on one department (no rip-and-replace) [company.md], [voice.md].
- Against open-source (OpenMRS/Bahmni): position free AND fully hosted with offline sync, removing the self-hosting and maintenance burden [competitors.md].
- Against Helium Health: compete on free pricing and offline resilience, not on track record [competitors.md].
- On "you are unproven": offer a free start with no lock-in so they verify before spending [customers.md].
- Do not overclaim HIPAA: use the homepage phrasing "Designed to satisfy HIPAA (US) security rules" [voice.md].
