# HIPAA controls and evidence (what "show me" looks like)

When a customer auditor, an OCR investigator or a cyber insurer says "show me", they do not want a policy. They want a **dated record, produced by the system or a person, that proves the control happened**. This file is the bridge from rule to proof.

## The three-step chain

```
 Control statement  ──►  Test procedure  ──►  Evidence
 "what we promise"      "how a stranger       "what they will
                         checks it"            actually look at"
```

- **Control statement:** one sentence, a verb, an owner and a frequency. "Workforce access to ePHI is reviewed quarterly by system owners."
- **Test procedure:** the steps an auditor follows. Pick a sample, compare to source, check sign-off date.
- **Evidence:** the artefact. A report export, a ticket, a log query, a signed form. It carries a date and a name.

Bad statement: "We take access control seriously." Good statement: "Every quarter the owner of `pl-prod-rcm` reviews the IAM Identity Center assignment report, removes unneeded access within 5 business days, and signs the review ticket."

## Design vs operating effectiveness

| Question | Design effectiveness | Operating effectiveness |
| --- | --- | --- |
| Asks | If followed, does the control reduce the risk? | Was it followed, every time, over the period? |
| Test | Read the policy and configuration | Sample real events across the period |
| Failure looks like | A policy with no owner or no frequency | A quarterly review done twice in a year |
| PulseLedger example | VDI policy with clipboard disabled (good design) | Chennai still had clipboard on in a test sample (not operating) |

A one-time screenshot proves design at best. SOC 2 Type II and similar reports test operation over a period, usually 6–12 months. Show both.

## The 15-row evidence table

| # | Control statement | HIPAA reference | Evidence |
| --- | --- | --- | --- |
| 1 | Workforce access reviewed quarterly | §164.308(a)(3)(ii)(B), (a)(4)(ii)(C) | IAM Identity Center assignment report + review ticket with sign-off date |
| 2 | Every user has a unique login; no shared accounts | §164.312(a)(2)(i) | IdP user export; exception register (the shared Riverbend EHR login, with closure date) |
| 3 | MFA on all admin and remote access | §164.312(d) | IdP policy export; MFA coverage report; legacy admin console fix ticket |
| 4 | ePHI encrypted at rest | §164.312(a)(2)(iv) | AWS Config rule `encrypted-volumes` compliance history; KMS key policies |
| 5 | Data events logged for ePHI buckets | §164.312(b) | CloudTrail trail config showing S3 data events; sample query result |
| 6 | Logs kept for the policy period, tamper-resistant | §164.312(b); §164.316(b)(2) | Log archive bucket with Object Lock; lifecycle config |
| 7 | Risk analysis done annually and on major change | §164.308(a)(1)(ii)(A) | Dated risk register, scope memo, leadership sign-off |
| 8 | Training completed by all workforce within 30 days of hire and yearly | §164.308(a)(5); §164.530(b) | LMS completion export per person, including contractors |
| 9 | Sanctions applied consistently | §164.308(a)(1)(ii)(C) | Sanction case log with outcomes (names redacted for audit) |
| 10 | Every PHI vendor has a signed BAA before access | §164.308(b); §164.502(e) | BAA register matched to vendor list; signed PDFs |
| 11 | Incidents assessed and documented | §164.308(a)(6); §164.414 | Incident file: timeline, four-factor memo, notice record |
| 12 | Backups restore successfully | §164.308(a)(7)(ii)(A)–(B) | AWS Backup job report; DR test report with restore time |
| 13 | Media and VDI disposal documented | §164.310(d)(2) | Destruction certificates; asset disposal log |
| 14 | Policies reviewed at least annually | §164.316(b)(2)(iii) | Version history with approval signature |
| 15 | Information system activity regularly reviewed | §164.308(a)(1)(ii)(D) | Weekly alert review ticket; anomaly alert tuning log |

Notice that each row can be satisfied by something a system produces. Where evidence depends on a human remembering, add a calendar reminder and a ticket template.

## Worked evidence: INC-2026-035 (the snooping case)

A Pune coder looked up the claim history of a local celebrity, a Riverbend patient, with no work reason. A new access-anomaly alert caught it. This single event is evidence for four controls:

| Control | What the case proves |
| --- | --- |
| #15 Activity review | The alert fired and a human triaged it within a defined time. Keep the alert, the triage ticket and the timestamps |
| #9 Sanctions | The coder was sanctioned per policy. Keep the HR record (restricted) and the case log entry |
| #11 Incident documentation | An impermissible use; four-factor assessment, notice to Riverbend per its BAA |
| #1 Access | Queue scoping lessons; was this coder's access broader than needed? |

An auditor can sample this case: "Show me the last access-anomaly alert and what you did." Having a complete file is worth more than saying you have no snooping. A program that never finds anything looks like one that never looks. Policy side: [../06_Security_Rule/risk_analysis_with_worked_example.md](../06_Security_Rule/risk_analysis_with_worked_example.md).

## Recognized security practices (the 12-month look-back)

Public Law 116-321 (the 2021 HITECH amendment) requires HHS to consider whether a CE or BA had **recognized security practices (RSPs)** in place for the previous 12 months when it decides on fines, audit outcomes and mitigation of remedies. It is not a safe harbor and not a certification. OCR's wording points to the NIST Cybersecurity Framework, the HHS 405(d) program (HICP) and similar programs developed under other statutes or regulations.

How to use it: keep **12 months of continuous evidence** that you follow a named framework. That means the table above, dated, with no gaps. If a regulator opens an investigation, send the evidence pack in your first reply. It can reduce the penalty and shorten the period of a corrective action plan.

## Evidence hygiene

- **Name the file for the control and the date:** `ctl-01_access-review_2026-Q3.pdf`.
- **Store in `pl-security`** (log-archive account), versioned, with Object Lock for items that must not change.
- **Keep six years** (§164.316(b)(2)(i)).
- **Redact personal data in evidence.** Evidence about PHI should not itself be PHI. Use counts and record IDs, not names.
- **Show failures too.** A review that found and fixed three bad accounts is better evidence than a review that found nothing.

## Try this

Write five control statements for **ScribeAssist**, each with a test procedure and its evidence. Suggested topics: BAA with the LLM provider exists and zero data retention is configured; dictation audio encrypted with a dedicated KMS key; prompts and outputs logged without leaking to LogPeak; no production audio in `pl-dev`; human reviewers have unique logins and quarterly access review. Link each to a Security Rule citation. See also [./running_a_hipaa_program.md](./running_a_hipaa_program.md) and [../12_AI_and_Health_Data/ai_with_phi.md](../12_AI_and_Health_Data/ai_with_phi.md).
