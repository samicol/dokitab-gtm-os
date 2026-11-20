# DokiTab Value Proposition

Grounded only in `context/docs/research/`. Persona names from `personas.md` [personas.md]. No em dashes.

## Economic Buyer (board / owner)

| Field | Detail | Source |
|---|---|---|
| Job to Be Done | Run the facility's systems at the lowest total cost and stop revenue leakage | [voice.md], [company.md] |
| Pain | Legacy per-seat licensing at "₦25,000 to ₦50,000 per user seat" and server-room CapEx of ₦12M to ₦22M; cashier/revenue leakage | [voice.md] |
| Gain | Free core tier, no per-seat fees, no server CapEx; board-ready reports | [company.md], [voice.md] |
| Proof | "100% Free (₦0/month, zero hidden charges)" with unlimited staff accounts; a CMD reports cutting HMO claim rejections from 18% to under 1.5% | [company.md], [customers.md] |

## Champion (CMD / consultant)

| Field | Detail | Source |
|---|---|---|
| Job to Be Done | Keep clinical records complete and compliant and keep HMO claims paid, through outages | [company.md], [customers.md] |
| Pain | HMO claim rejections; "losing records to paper folders"; clinical downtime during "NEPA" blackouts | [customers.md], [voice.md] |
| Gain | One offline-first record, an HMO claims module, an immutable audit trail, and NDPA 2023 alignment | [company.md] |
| Proof | Testimonial "Cut our quarterly HMO claim rejections from 18% to under 1.5%," (CMD, Ikeja, Lagos); offline SQLite replay of vitals, MAR doses, and SOAP notes on reconnect | [customers.md], [company.md] |

## Influencer (pharmacist, lab scientist, billing/claims officer, nurse, cashier)

| Field | Detail | Source |
|---|---|---|
| Job to Be Done | Dispense, run the lab, submit claims, and chart accurately day to day | [company.md] |
| Pain | Drug expiry and stock-outs; paper folders at the point of care; charting safely on personal phones ("BYOD Nurse Workflows") | [company.md], [voice.md] |
| Gain | FEFO dispensing with expiry and reorder alerts; lab panic-value alerts and HL7 analyzer feeds; clean HMO claim submission; biometric-locked BYOD charting | [company.md] |
| Proof | Named modules: FEFO batch dispensing, HL7 feeds (Mindray, Sysmex, Roche), controlled-drugs register; security controls (biometric lock after 2 minutes, OS-level screenshot block, AES-256 offline queue) | [company.md] |
