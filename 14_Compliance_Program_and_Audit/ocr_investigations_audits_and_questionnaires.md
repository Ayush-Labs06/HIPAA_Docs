# OCR investigations, audits and customer security questionnaires

Three different people can ask "show me you comply": a regulator (OCR), an auditor, and a customer's security team. They want the same evidence, but the stakes differ. OCR can fine you. A customer can walk away. This file covers both, using PulseLedger.

## How an OCR investigation works

OCR is the HHS Office for Civil Rights. It enforces the Privacy, Security and Breach Notification Rules (45 CFR Parts 160 and 164). Business associates (BAs) are directly liable since HITECH, so OCR can investigate PulseLedger itself, not only its customers.

```
Trigger ──▶ OCR opens matter ──▶ Data request letter ──▶ Review
  │                                    (typically 30 days to reply)
  ├─ complaint (anyone)                                      │
  ├─ breach report (500+ always reviewed)         ┌──────────┼───────────────┐
  └─ compliance review (OCR's own initiative)     ▼          ▼               ▼
                                          Close / technical  Resolution     Notice of Proposed
                                          assistance         agreement +    Determination ──▶ CMP
                                                             CAP (payment)  (rare, contested)
```

The outline follows OCR's public description of its process. Reply deadlines are set in the letter you receive; it governs.

| Outcome | What it means | Typical shape |
| --- | --- | --- |
| Technical assistance / no violation | OCR closes after the entity fixes things | No payment |
| **Resolution agreement** (RA) | A negotiated settlement, no admission of liability | Payment plus a **corrective action plan** (CAP) |
| **Civil money penalty** (CMP) | OCR imposes a penalty after a formal process | Per-violation tiers; see [ocr_penalties_and_how_enforcement_works.md](../16_Enforcement_and_Case_Law/ocr_penalties_and_how_enforcement_works.md) |

**CAPs** usually run two to three years and often require a fresh enterprise-wide risk analysis, updated policies, workforce training and periodic reports to OCR. Read the specific agreement; do not assume.

## What OCR asks for (and what that tells you to keep)

A data request letter typically asks for:

- The most recent **risk analysis** and risk management plan (§164.308(a)(1)(ii)(A)-(B))
- Policies and procedures, with dates and owners (§164.316)
- Training records (§164.308(a)(5), §164.530(b))
- BAAs for the vendors involved (§164.504(e))
- Incident and breach files, including the four-factor assessments
- Evidence of audit-log review and access control (§164.312(b), §164.308(a)(1)(ii)(D))

PulseLedger's as-is state fails several of these on sight: a 2021 checklist as "risk analysis", policies from 2019, no Chennai training records, no BAAs for LogPeak or the speech-to-text vendor. Evidence built in [controls_and_evidence.md](./controls_and_evidence.md) is what you would hand over.

## The OCR audit program

OCR may audit under HITECH §13411, separately from complaints. Audits are not a response to a violation.

- **2016-2017 (Phase 2):** desk audits of covered entities and business associates. OCR later published findings; the consistent weak spots were **risk analysis**, **risk management** and **right of access** processes. I did not retrieve the published figures: **check current status** on the OCR audit page (hhs.gov/hipaa, "audit" section) before quoting numbers.
- **2024-2025 round:** OCR launched a new round in late December 2024, covering **50 covered entities and business associates**, focused on selected Security Rule provisions most relevant to hacking and ransomware. This followed a November 2024 HHS OIG report urging OCR to widen audit scope. As of November 2026 I found **no published OCR industry report** from this round; OCR has said it will publish one. Treat results as "check current status".
- **Not verified:** whether OCR announced a further round in 2026, and how any HHS reorganisation affects OCR's audit capacity. **Check current status.**

Related OCR priorities: the **Risk Analysis Initiative**, ransomware investigations and the **Right of Access Initiative**. Counts change often; do not hard-code them.

## Real enforcement (kept small and verified)

These come from OCR's public resolution agreements page. Confirm each there before quoting.

| Case | Year | Amount | Lesson |
| --- | --- | --- | --- |
| Anthem | 2018 | US$16 million | Large breach, no enterprise-wide risk analysis. The risk analysis is the first thing OCR asks for |
| Premera Blue Cross | 2020 | US$6.85 million | Same pattern: risk analysis gaps and weak audit controls before a breach |

More cases, with module links, are in [big_cases_and_lessons.md](../16_Enforcement_and_Case_Law/big_cases_and_lessons.md). I am deliberately not citing 2025-2026 settlements from memory; use the OCR page.

## Customer security questionnaires

OCR is rare. Questionnaires are weekly. Summit Valley Health Plan (a Medicare Advantage prospect) sent **280 questions** plus a CMS offshore-subcontractor attestation. Jason Pike wants the deal; Dr. Morgan and Marcus Webb own the sign-off.

### Build an answer library, not one-off answers

1. Export every past questionnaire (SIG, CAIQ, HECVAT-style, bespoke) into one sheet.
2. Normalise to one canonical answer per control topic: ID, question pattern, answer, status (true today / planned with date), evidence link, owner, last reviewed.
3. Review quarterly. An answer older than 12 months is stale.
4. One person (Priya) edits; owners approve their domain; legal approves anything about liability, offshore or certifications.

### Do not overclaim

- Never write "HIPAA compliant" as a blanket claim. There is no HIPAA certification, and nobody can certify PulseLedger. Say "PulseLedger operates as a business associate and maintains a HIPAA security and privacy program; here is our latest risk analysis date and scope."
- Never answer in the future tense as if it were true. Say "by [date]" and attach the plan.
- Give SOC 2 or HITRUST only if you hold the report; state the scope and period.

### Ten tough questions, answered honestly (as-is, January 2026)

| # | Question | Honest answer | Evidence / next step |
| --- | --- | --- | --- |
| 1 | Date of your last enterprise-wide risk analysis? | The prior one (2021) was a questionnaire checklist and excluded ScribeAssist, PulseWell and Chennai. A full analysis is under way | Share the scope and the target date, see [risk_analysis_with_worked_example.md](../06_Security_Rule/risk_analysis_with_worked_example.md) |
| 2 | Do you sign BAAs with all subcontractors? | Not yet. Missing: intercompany (US Inc and India Pvt Ltd), LogPeak, speech-to-text, staffing firm | Remediation tracker with dates, see [vendor_and_cloud_provider_management.md](../08_Business_Associate_Agreements_and_Vendors/vendor_and_cloud_provider_management.md) |
| 3 | Is MFA enforced for all admin access? | Not for the legacy LedgerRCM admin console | Fix date; the compensating control meanwhile |
| 4 | Is PHI encrypted at rest? | Yes in current services; one legacy EBS volume with 2019-2021 claims is unencrypted and is being migrated | Config evidence plus ticket |
| 5 | How long do you retain audit logs? | Application logs 30 days today. Moving to 6 years for documentation-type evidence and a defined period for logs | See [retention_requirements.md](../10_Retention_Disposal_and_Media/retention_requirements.md) |
| 6 | Do offshore staff access PHI? | Yes, via a VDI in `us-east-1`. Clipboard and print redirection controls are being tightened | Attestation answers in [offshoring_phi_to_india.md](../09_Offshore_and_India/offshoring_phi_to_india.md) |
| 7 | Breach history in the last 3 years? | Disclose incidents that met the definition and how they were handled | Counsel approves wording; see [is_it_a_breach.md](../07_Breaches_and_Incidents/is_it_a_breach.md) |
| 8 | Do you train all workforce, including contractors? | Annual training; completion records for Chennai contractors are missing and being rebuilt | Training LMS export |
| 9 | Do you use PHI to train AI models? | Not without customer authorisation in the BAA; ScribeAssist's LLM vendor has a BAA with zero data retention | See [ai_with_phi.md](../12_AI_and_Health_Data/ai_with_phi.md) |
| 10 | Do you hold a HIPAA seal or badge? | No such certification exists (see above). We can show our risk analysis, policies and assessments | Do not claim otherwise |

## Common mistakes

- Treating an answer as marketing. It can become a contract warranty.
- Answering "yes" to the control as designed, while it does not operate (design vs operating effectiveness).

## Try this

Take five items from the table above (pick #1, #2, #3, #6 and #8) and write the answer you would send Summit Valley, each in two sentences: what is true today, and what date the gap closes. Add a column for the evidence file you would attach. Then list which of the five answers needs Dr. Morgan or Marcus Webb to approve before sending.
