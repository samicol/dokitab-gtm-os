# DokiTab Buyer Personas

Built only from facts verified in `context/docs/research/` (checked 2026-11-20). Pains use verbatim research phrasing in quotation marks where present; synthesized phrasing is marked [INFERRED FROM RESEARCH]. Every line cites its source file.

---

## Persona 1: Champion

| Field | Detail | Source |
|---|---|---|
| Job titles | Chief Medical Director (CMD); consultant | [customers.md], [company.md] |
| Measured on | HMO claim rejection rate; completeness and retrieval of clinical records; clinical uptime during outages; regulatory compliance (NDPA, HEFAMAA, PCN) | [customers.md], [company.md], [voice.md], [market.md], [signals.md] |

**Top three pains (in their own words):**
1. "Cut our quarterly HMO claim rejections from 18% to under 1.5%," indicates that high HMO claim rejections were the starting pain [customers.md] (figures verbatim; pain framing [INFERRED FROM RESEARCH]).
2. "losing records to paper folders" [voice.md] (verbatim).
3. Clinical work stalls during "NEPA" blackouts [voice.md] ("NEPA" verbatim; sentence [INFERRED FROM RESEARCH]).

**What makes them start looking:** new HMO onboarding or a spike in claim rejections [signals.md], [market.md]; an upcoming HEFAMAA or PCN inspection or a recent sanction [signals.md]; NDPA registration or a data-protection audit [signals.md].

**Common objections and counter-angles:**
- "Paper-to-digital is a big training burden" [market.md] → 5-minute plug-and-play standalone mode and a free tier to pilot one department first [company.md].
- "Where does patient data live, is it secure?" → in-country AWS Lagos Local Zone residency, AES-256 encrypted offline queue, TLS 1.3, immutable audit log, NDPA 2023 alignment [company.md].
- "You are an unproven vendor with no references" [customers.md] → free core tier with no per-seat fees lets them verify before spending [company.md] [INFERRED FROM RESEARCH].

---

## Persona 2: Economic Buyer

| Field | Detail | Source |
|---|---|---|
| Job titles | Facility board (named as the "board-ready reports" audience); owner/proprietor [INFERRED FROM RESEARCH] | [company.md] |
| Measured on | Total cost of ownership; revenue leakage at the till; HMO claim revenue recovered; cost per seat and server CapEx avoided | [voice.md], [customers.md], [company.md] |

**Top three pains (in their own words):**
1. Legacy licensing at "₦25,000 to ₦50,000 per user seat" [voice.md] (verbatim).
2. "₦12M–₦22M" server-room CapEx [voice.md] (verbatim).
3. "cashier leakage" draining till revenue [voice.md] (term is paraphrase in research, so [INFERRED FROM RESEARCH]).

**What makes them start looking:** a legacy renewal or price increase [voice.md] [INFERRED FROM RESEARCH]; a new branch expansion that multiplies per-seat cost [signals.md]; board pressure on cost and revenue leakage [company.md] [INFERRED FROM RESEARCH].

**Common objections and counter-angles:**
- "If the core is free, what is the catch?" → "100% Free (₦0/month, zero hidden charges)" and "no expiry and no per-seat fees"; paid items are explicit (₦30,000/month plan, SMS at ₦5, WhatsApp at ₦12, 0.5% gateway fee) [company.md], [voice.md].
- "Is a free product and vendor viable long term?" (funding Not found) [customers.md] → no server CapEx and no per-seat lock-in lower switching risk [company.md] [INFERRED FROM RESEARCH].
- "Can it govern our multi-branch group?" → multi-branch governance on the ₦30,000/month plan [company.md].

---

## Persona 3: Influencer

| Field | Detail | Source |
|---|---|---|
| Job titles | Pharmacist (PCN-regulated premises); lab scientist; billing/claims officer; health records officer; IT/EMR administrator; nurse; cashier (signals.md roles are inferred digitization-signal roles) | [market.md], [company.md], [signals.md], [voice.md] |
| Measured on | Dispensing accuracy and expiry control (FEFO, reorder alerts); lab turnaround and panic-value alerts; claim submission accuracy; record retrieval; till reconciliation | [company.md], [customers.md], [voice.md] |

**Top three pains (in their own words):**
1. Drug expiry and stock-outs without batch tracking [company.md] [INFERRED FROM RESEARCH] (FEFO, expiry tracking, reorder alerts are the features that address it).
2. "losing records to paper folders" at the point of care [voice.md] (verbatim).
3. Charting securely on personal phones, framed as "BYOD Nurse Workflows" [voice.md] (title verbatim; pain [INFERRED FROM RESEARCH]).

**What makes them start looking:** a PCN or HEFAMAA inspection that requires proper records [signals.md], [market.md]; a new lab analyzer needing HL7 integration [company.md]; a management mandate after claim rejections [customers.md] [INFERRED FROM RESEARCH].

**Common objections and counter-angles:**
- "Personal devices are not safe for patient data" → biometric lock after 2 minutes, OS-level screenshot blocking, GPS geofencing, encrypted offline queue [company.md].
- "Another system to learn" [market.md] → irrelevant screens auto-hide and each department can run as a standalone plug-and-play module [company.md].
- "It will not work when power or internet drops" → offline-first; vitals, MAR doses, and SOAP notes replay on reconnect [company.md], [voice.md].
