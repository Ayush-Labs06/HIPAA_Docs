# Running a HIPAA compliance program (at a business associate)

HIPAA is not a project that ends. It is a set of habits that must be *provable* six years later. A program is how you turn the rules into habits: owners, a calendar, records, and a board that reads the right numbers.

This file is for a business associate (BA), which is PulseLedger's position. A covered entity (CE) has extra duties (notice of privacy practices, patient rights desk), but the skeleton is the same.

## The components

| Component | Rule | PulseLedger owner | Proof you keep |
| --- | --- | --- | --- |
| Designated officers | §164.308(a)(2); §164.530(a) | Dr. Leah Morgan (Privacy), Rohan Kulkarni (Security) | Written designation, org chart |
| Policies and procedures | §164.316(a); §164.530(i) | Priya Nair drafts; officers approve | Versioned policies, approval record |
| Risk analysis and risk management | §164.308(a)(1)(ii)(A)–(B) | Rohan | Dated risk register, treatment plan |
| Workforce training | §164.530(b); §164.308(a)(5) | Priya | Completion records per person, content, date |
| Sanctions | §164.530(e); §164.308(a)(1)(ii)(C) | Dr. Morgan with HR | Sanction policy, case log |
| Access management and reviews | §164.308(a)(3)–(4) | Rohan, system owners | Quarterly review sign-offs |
| Business associate agreements | §164.502(e); §164.504(e); §164.308(b) | Marcus Webb; Priya tracks | BAA register |
| Incident and breach response | §164.308(a)(6); §164.410 | Rohan, Dr. Morgan | Incident file per case |
| Contingency planning | §164.308(a)(7) | Sneha Joshi | DR test report |
| Evaluation (periodic technical and non-technical) | §164.308(a)(8) | Priya | Audit reports |
| Documentation and 6-year retention | §164.316(b)(2); §164.530(j) | Priya | Retention schedule, repository |

See [../06_Security_Rule/risk_analysis_with_worked_example.md](../06_Security_Rule/risk_analysis_with_worked_example.md) and [../01_Scope_and_Roles/privacy_and_security_officers.md](../01_Scope_and_Roles/privacy_and_security_officers.md).

## A structure from the OIG

The HHS Office of Inspector General published its **General Compliance Program Guidance (GCPG)** in November 2023 (6 November 2023). It is voluntary and aimed at the wider health care industry, but it gives you a ready outline the board will recognise: written standards, compliance leadership and oversight, training, effective lines of communication, enforcement through discipline, risk assessment with auditing and monitoring, and responding to problems with corrective action. Read the guidance itself before citing element names or numbers in front of a customer.

## The annual calendar (PulseLedger, 12 months)

| Month | Activity | Output |
| --- | --- | --- |
| Jan | Annual risk analysis kick-off (scope: all ePHI, including ScribeAssist and Chennai) | Scope memo |
| Feb | New-year training launch; policy review cycle begins | Training roster |
| Mar | Risk analysis results to leadership | Risk register v1 |
| Apr | Quarterly access review #1; BAA renewals due | Review sign-offs |
| May | Vendor tiering refresh | Vendor register |
| Jun | Mid-year policy attestation; tabletop exercise | Tabletop report |
| Jul | Quarterly access review #2; DR test (us-west-2 failover) | DR report |
| Aug | Offshore access review (Pune, Chennai) | Offshore assessment update |
| Sep | Internal audit of controls; sanctions log review | Audit report |
| Oct | Quarterly access review #3; board security and privacy report | Board pack |
| Nov | Customer-audit readiness; questionnaire answer library refresh | Answer library |
| Dec | Annual evaluation (§164.308(a)(8)); year-end breach log check (the <500 log is due to HHS within 60 days after year end) | Evaluation memo |

Note the year-end item: for breaches of fewer than 500 people, a CE reports to HHS within 60 days after the end of the calendar year. As a BA, PulseLedger feeds the facts to each CE; see [../07_Breaches_and_Incidents/breach_notification_60_days.md](../07_Breaches_and_Incidents/breach_notification_60_days.md).

## Metrics a board actually reads

Boards do not read control lists. They read ten numbers and the trend.

| Metric | Target | Why |
| --- | --- | --- |
| Open critical/high risks past due date | 0 | Shows the plan is real |
| Workforce training completion (incl. Chennai contractors) | 100% within 30 days of hire/annual | The old gap |
| Access reviews completed on time | 100% | §164.308(a)(3) evidence |
| Vendors with PHI and a signed BAA | 100% | Currently four vendors below this |
| Mean time to detect / contain incidents | Trend down | Detection quality |
| Incidents by category and number of people affected | Trend, not a goal | INC-2026-019 to -035 |
| Customer BAA notice clocks met | 100% | Contract risk |
| Unencrypted ePHI stores found | 0 | Closed once the legacy EBS volume is fixed |
| Audit-log retention coverage | Meets policy | Fix of the 30-day gap |
| Customer questionnaires answered within SLA | Target set with Jason Pike | Revenue link |

## Making it run

1. **One owner per row**, a named person, not a team. Teams do not get sanctioned.
2. **Evidence by default.** Every control produces a record without anyone remembering to make one. See [./controls_and_evidence.md](./controls_and_evidence.md).
3. **Write exceptions down.** A signed exception with an end date beats an unwritten workaround. The Chennai VDI clipboard rule is a case in point.
4. **Close the loop after incidents.** INC-2026-035 (insider snooping, found by an access-anomaly alert) fed three changes: a sanction case, a new alert rule and a training scenario.
5. **Keep the six years.** Policies, risk analyses, training records, BAAs, incident files and review sign-offs are all "documentation" under §164.316(b)(2). Retention schedule: [../10_Retention_Disposal_and_Media/retention_requirements.md](../10_Retention_Disposal_and_Media/retention_requirements.md).

## Real enforcement

OCR announced a **Risk Analysis Initiative** in October 2024, aimed at failures to perform an accurate and thorough risk analysis. Sources I found counted at least twelve enforcement actions under it by late 2025; check OCR's enforcement highlights page for the current figure. The lesson for a program: an annual calendar item that says "risk analysis" is worthless without a dated, scoped, completed document. Cases are in [../16_Enforcement_and_Case_Law/big_cases_and_lessons.md](../16_Enforcement_and_Case_Law/big_cases_and_lessons.md).

## Try this

Build PulseLedger's 12-month calendar for **2027** in a spreadsheet. For each activity add: owner, HIPAA citation, evidence artefact, due date, and the board metric it feeds. Then mark the three items you would do first if you had only 30 days.
