# Copy: New Facility (facility-expansion)

**Status:** COPY-APPROVED 2026-11-21 · S1 x Champion (CMD) cell · email + LinkedIn
Spec: [workflow-spec.md](workflow-spec.md). Written with the `campaign-copy` skill.

**Scope:** base sequence for the primary cell S1 (multi-specialty hospital + diagnostics) x Champion (CMD). Other segment/persona cells are parallel variants to add later off the same skeleton.

**Facts note:** `context/messaging/facts.md` does not exist, so facts were sourced from `messaging.md`, `value-proposition.md`, and `context/docs/research/`. No named customer and no unverified metric (the anonymous 18% to 1.5% claim) appear in the copy. Citations below are in notes only, never in prospect-facing lines.

**Bans honored:** no em/en dashes, no `!`, no "free" (rendered as "₦0 a month / no per-seat fees"), no "congrats / I saw / noticed / looks like" openers [copy-bans.yaml].

**Updated 2026-11-21:** the e1l1 waterfall was re-pointed from `is_new_hefamaa` to `is_new_opening` / `is_new_listing` to match the revised trigger architecture in `workflow-spec.md` (Places snapshot delta + opening announcements; HEFAMAA is now a verification flag, not a trigger).

---

## Email 1

**Subject options:** `records before you lock in` | `one record for {{facility_name}}` | `charting through power cuts`

```
{{first_name}},

{{e1l1}}

{{e1l2}}

DokiTab {puts wards, pharmacy, lab and billing on one record|runs the whole facility on one record} that {stays up when the power and internet drop|keeps working through power cuts}, at ₦0 a month with no per-seat fees.

{Worth setting up a workspace before the records system is locked in?|Want to try it on one department before you commit?}
```

**`{{e1l1}}`** (routes on trigger; `is_new_opening` takes priority over `is_new_listing`, and one is always set by construction):
- `+ is_new_opening:` Opening {{facility_name}} in {{city_area}} usually means the records and billing setup is still being decided.
- `+ is_new_listing:` A new facility like {{facility_name}} in {{city_area}} usually means the records system is still an open decision.
- `FALLBACK:` Standing up {{facility_name}} in {{city_area}} usually means the records system is still an open decision.

**`{{e1l2}}`** (routes on `bills_hmo`, boolean = exhaustive, no fallback):
- `+ bills_hmo = true:` With {{hmo_name}} billing in the mix, clean claims from the first encounter tend to matter more than people expect early on.
- `+ bills_hmo = false:` Before patient volume builds, getting records and the audit trail right once is easier than fixing paper habits later.

Notes: value prop from ₦0/no-per-seat and offline-first [company.md], [voice.md]; HMO-claims capability [company.md]. No social-proof line: no authorized matched proof exists (Law 4).

---

## Email 2 (same thread, Re: same subject)

Opens straight on new value (parallel pilot + a concrete offline detail E1 did not state). Static.

```
{{first_name}},

You can run DokiTab on one department alongside whatever records you use now, so there is no rip-and-replace to start.

When the power or internet drops, vitals, drug charts and notes stay on the device and sync back on reconnect.

{Want a short look at how the one-department setup works?|Should I show you the pilot setup?}
```

Notes: standalone plug-and-play / per-department run [company.md]; offline queue replays vitals, MAR, notes on reconnect [company.md], [voice.md].

---

## Email 3 (same thread, breakup + low-friction PLG)

```
{{first_name}},

I will leave it here so I am not cluttering your inbox.

If settling {{facility_name}}'s records is still on the list, the quickest way to judge it is to spin up a workspace and try one department yourself.

{Want the link?|Should I send the link?}
```

Notes: breakup close (Law 10); PLG self-serve workspace CTA [company.md], [gtm-system.md]. Link sent on reply, not in the cold body.

---

## LinkedIn (2 DMs, parallel to email)

**Touch 0 (connection request):** empty, no note [linkedin-dm-frameworks.md].

**DM 1** (1 to 2 days after accepted; one forward-looking question, answerable to a stranger):
```
{{first_name}}, as you get {{facility_name}} up and running, are you settling the records and billing system now or after you open?
```

**DM 2** (3 to 5 days after DM 1; context + soft offer, static, no link):
```
Asking because the whole point of DokiTab is getting wards, pharmacy, lab and billing on one record from day one, at ₦0 a month with no per-seat fees. Happy to show what that looks like if it is useful.
```

No DM 3 [linkedin-dm-frameworks.md]. LinkedIn leads with the question; email leads with the situation, so the two channels do not duplicate.

---

## Variables used (all in the spec's variables contract, section 6)

| Variable | Used in | Source field |
|---|---|---|
| `{{first_name}}` | all greetings, DMs | Apollo/Prospeo |
| `{{facility_name}}` | E1 (l1, subject), E3, DM1 | Places/HEFAMAA |
| `{{city_area}}` | E1 l1 (all branches) | Google Places |
| `{{hmo_name}}` | E1 l2 (bills_hmo=true only) | HMO/ILERA EKO directory |

---

## Assembled sample email (S1, is_new_opening, bills_hmo = true)

```
Subject: records before you lock in

{{first_name}},

Opening {{facility_name}} in {{city_area}} usually means the records and billing setup is still being decided.

With {{hmo_name}} billing in the mix, clean claims from the first encounter tend to matter more than people expect early on.

DokiTab puts wards, pharmacy, lab and billing on one record that stays up when the power and internet drop, at ₦0 a month with no per-seat fees.

Worth setting up a workspace before the records system is locked in?
```

QA: passed on every unit. insult ok, nexus ok, facts-traced ok (no unverified metric, no named customer), data-must-exist ok, spintax ok (none in dynamic lines), follow-ups add new value, subject constant across thread, no banned words, no "free". Word counts: E1 71, E2 48, E3 40, DM1 24, DM2 46.
