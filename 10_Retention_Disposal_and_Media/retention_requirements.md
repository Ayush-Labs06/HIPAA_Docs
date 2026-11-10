# Retention: what HIPAA requires (and the myth it doesn't)

The most repeated HIPAA myth: "HIPAA says keep medical records for six years." It does not. This file separates the three different clocks people mix up, then builds a retention schedule for PulseLedger.

## Three clocks, three sources

| Clock | What it covers | Source | Length |
| --- | --- | --- | --- |
| **HIPAA documentation** | Policies, procedures, risk analyses, training records, BAAs, breach assessments, sanctions, complaints, accounting logs, authorizations, NPP versions | §164.316(b)(2)(i) (Security Rule); §164.530(j)(2) (Privacy Rule); §164.414(b) (burden of proof in breach cases, so keep the breach assessment) | **6 years** from creation or last effective date, whichever is later |
| **Medical-record retention** | The clinical record itself | State law, Medicare conditions of participation, payer contracts | Varies. For example 42 CFR 482.24(b)(1) requires hospitals to keep records for at least **5 years**; Medicare Advantage organizations must retain records for **10 years** (42 CFR 422.504(d)). Many states set longer periods, and minors' records often run to a set age plus years. **Check current status** for every state you serve |
| **BA contract end** | PHI a business associate holds for a customer | BAA, §164.504(e)(2)(ii)(J) | **Return or destroy** at termination, if feasible |

Key points:

- HIPAA sets **no** minimum retention for medical records. It sets how long you keep *your compliance paperwork*.
- HIPAA also does not tell you when to delete PHI. But "minimum necessary" and the BAA do. A BA holding PHI longer than the contract allows is using it outside the BAA.
- The CE owns the medical record. PulseLedger holds a *copy* for billing work and must follow the CE's retention instruction.

## The BA position: return or destroy

The BAA must say that at termination the BA will **return or destroy** all PHI it still maintains, if feasible (§164.504(e)(2)(ii)(J)). If it is **not feasible**, the BA extends the BAA's protections to that PHI and limits further use to the purposes that make return or destruction infeasible.

When is it truly infeasible? Examples that regulators and lawyers accept:

- A legal hold or an open audit that requires records to stay.
- State or federal law that requires the BA to keep claim records.
- Backups that cannot be selectively edited until they expire (with a written expiry plan).

"It's hard" is not infeasible. Write the infeasibility reason, the protections that continue, and the date it will end. See [baa_clause_by_clause_review.md](../08_Business_Associate_Agreements_and_Vendors/baa_clause_by_clause_review.md) for clause wording.

## PulseLedger retention schedule (proposed)

Owner: Priya Nair. Approver: Dr. Leah Morgan. Where a customer BAA is stricter, the BAA wins. Review each year.

| Record | System / account | Retention | Trigger and reason | Disposal method |
| --- | --- | --- | --- | --- |
| Claims archive 2019-2021 | Legacy EC2 MySQL host, **unencrypted EBS** | Per customer BAA; default **return or destroy within 90 days of termination**; keep only if a customer's instruction or law requires | Contract end. First move: encrypt, then migrate or delete | Snapshot copy encrypted, delete original, KMS key schedule (see [disposal_and_media_reuse.md](./disposal_and_media_reuse.md)) |
| Active claims and remittances | `pl-prod-rcm` S3 / RDS | Life of contract, then 90 days | BAA termination | S3 lifecycle + key deletion |
| Dictation audio (ScribeAssist) | `pl-prod-scribe` S3 | **30 days** after the note is signed, unless the customer asks for longer in writing | Audio is only an input; the signed note is the record | S3 lifecycle expiry |
| Draft transcripts and LLM output | `pl-prod-scribe` | 90 days or customer rule | Edit and audit window | Lifecycle expiry |
| **Dictation audio copies in `pl-dev`** | `pl-dev`, bucket `pl-scribe-audio-dev` | **0 days.** Should not exist | Dev data should be synthetic. INC-2026-024 exposed 1,850 people | Purge runbook in the disposal file |
| **Application audit logs** | App logs / CloudWatch | **Fix:** 12 months searchable, then archive to the `pl-security` account (Object Lock) for **6 years** | Currently 30 days. See below | Archive tier expiry after 6 years |
| CloudTrail incl. S3 data events | `pl-security` log archive | 12 months hot, **6 years** archive | Supports investigations, accounting of disclosures | Object Lock expiry |
| Indian-system logs (Pune, Chennai endpoints, firewall) | India log store | At least **180 days in India** (CERT-In Directions 2022) | Longer if the HIPAA schedule needs it | Lifecycle expiry |
| BAAs (signed) and amendments | Contract repository | 6 years after the BAA ends | §164.316(b)(2)(i) | Review then delete |
| Risk analyses and risk registers | Compliance repository | 6 years after superseded | §164.316(b)(2)(i), OCR asks for prior years | Review then delete |
| Policies and procedures | Compliance repository | 6 years after retired | Same | Review then delete |
| Training records | LMS | 6 years | §164.530(j); §164.308(a)(5) | Review then delete |
| Incident and breach files | Case tool | 6 years after closure; litigation hold overrides | §164.414, §164.530(j) | Review then delete |
| Sanction records | HR | 6 years | §164.308(a)(1)(ii)(C) | Review then delete |
| Chat tool messages | Company chat | Policy: no PHI; retain 1 year; purge flagged PHI messages after incident review | INC-2026-027: PHI pasted into a chat tool with no BAA | Admin purge with export evidence |

## Why 30 days of audit logs fails

HIPAA does not name a log retention number. But:

1. Audit controls (§164.312(b)) and information-system activity review (§164.308(a)(1)(ii)(D)) only work if the logs exist when you need them.
2. Breach discovery is often slow. In INC-2026-024, S3 server access logs were off, so nobody could show whether the bucket was read. With 30 days of application logs, anything older than a month is gone.
3. Accounting of disclosures (§164.528) looks back **6 years**. A BA that cannot show disclosures cannot support its customers.
4. Documentation that you reviewed logs (not the raw logs) must be kept 6 years.

A defensible position: **12 months searchable plus 6 years archived** for security-relevant audit logs, with PHI scrubbed from logs (names and MRNs should not be in them at all; the LogPeak issue in [risk_analysis_with_worked_example.md](../06_Security_Rule/risk_analysis_with_worked_example.md)). If cost is the objection, move logs to cheaper S3 storage classes rather than shortening retention. State the reasoning in the schedule so the number survives an audit.

## Common mistakes

- Quoting "HIPAA requires 6 years of medical records" to a customer. Say instead: "HIPAA requires six years for compliance documentation; record retention follows state law and your contract."
- Keeping everything forever "just in case". Old PHI is attack surface and a breach-size multiplier (61,400 people in INC-2026-031 included stale exports).
- Deleting documentation on a legal hold.
- Forgetting backups when promising deletion.

## Try this

Fix the **30-day audit-log retention**. Write a half-page memo to Rohan Kulkarni that proposes the new number, names the HIPAA provisions it supports, estimates the S3 storage cost of the archive tier for a plausible log volume (state your assumption), and lists what must be scrubbed from logs first. Add one row to the schedule above for a record type that is missing.
