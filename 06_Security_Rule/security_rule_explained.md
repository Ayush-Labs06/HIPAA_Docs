# The Security Rule explained (§164.302–§164.318)

The Security Rule (45 CFR Part 164, Subpart C) protects **electronic PHI (ePHI)**. It is about keeping ePHI confidential, intact and available. It applies to covered entities (CEs) **and** business associates (BAs) directly, so PulseLedger is on the hook in its own right, not only through contracts.

## The idea: flexible, not vague

§164.306(a) sets four goals: ensure confidentiality, integrity and availability; protect against reasonably anticipated threats; protect against reasonably anticipated impermissible uses; ensure workforce compliance.

§164.306(b) lets you pick controls using four factors: your **size, complexity and capability**; your **technical infrastructure**; the **cost** of controls; and the **probability and criticality** of the risk. Cost never excuses doing nothing. It only helps you choose between adequate options.

## Required vs addressable

Each standard has implementation specifications. Each is **Required (R)** or **Addressable (A)**.

- **R**: do it.
- **A**: assess whether it is reasonable and appropriate. If yes, implement. If not, **document why** and implement an equivalent alternative, or document why nothing is needed (§164.306(d)(3)).

Addressable does **not** mean optional. "We chose not to encrypt" without a written analysis is a finding.

## All standards in one table

| Standard | Implementation specification | R/A |
| --- | --- | --- |
| **Administrative** §164.308 | | |
| (a)(1) Security management | Risk analysis · Risk management · Sanction policy · Information system activity review | R · R · R · R |
| (a)(2) Assigned responsibility | Designated Security Official | R |
| (a)(3) Workforce security | Authorization/supervision · Clearance · Termination procedures | A · A · A |
| (a)(4) Information access | Isolating clearinghouse functions · Access authorization · Access establishment and modification | R · A · A |
| (a)(5) Awareness and training | Reminders · Malware protection · Log-in monitoring · Password management | A · A · A · A |
| (a)(6) Incident procedures | Response and reporting | R |
| (a)(7) Contingency plan | Data backup · Disaster recovery · Emergency mode · Testing and revision · Criticality analysis | R · R · R · A · A |
| (a)(8) Evaluation | Periodic technical and non-technical evaluation | R |
| (b)(1) BA contracts | Written contract | R |
| **Physical** §164.310 | | |
| (a)(1) Facility access | Contingency operations · Facility security plan · Access control and validation · Maintenance records | A · A · A · A |
| (b) Workstation use / (c) Workstation security | (no sub-specs) | R / R |
| (d)(1) Device and media | Disposal · Media re-use · Accountability · Backup and storage | R · R · A · A |
| **Technical** §164.312 | | |
| (a)(1) Access control | Unique user ID · Emergency access · Automatic logoff · Encryption | R · R · A · A |
| (b) Audit controls | (no sub-specs) | R |
| (c)(1) Integrity | Mechanism to authenticate ePHI | A |
| (d) Authentication | Person or entity authentication | R |
| (e)(1) Transmission security | Integrity controls · Encryption | A · A |
| **Organizational** §164.314 | BA contract content (a) · Group health plan (b) | R |
| **Documentation** §164.316 | Policies and procedures · Documentation · Availability · Updates | R |

## Documentation and retention (§164.316)

Policies must be written (or electronic). Keep documentation of policies, risk analyses, decisions and actions **6 years from creation or last effective date** (§164.316(b)(2)(i)). Make it available to those who use it. Review and update when the environment changes. PulseLedger's 2019 policies, never reviewed, fail the update requirement.

## The 2025 NPRM (what may change)

HHS published a Notice of Proposed Rulemaking on **6 January 2025** (90 FR 898). Headlines of the proposal:

- Remove the addressable/required split: everything is required, with narrow exceptions.
- Written inventory of technology assets and a network map, reviewed at least yearly.
- Multi-factor authentication and encryption at rest and in transit as defaults.
- Restore critical systems within **72 hours**; vulnerability scanning every 6 months and annual penetration testing.
- Annual compliance audit; BAs verify technical safeguards yearly and report contingency-plan activation fast.

**Status, as of November 2026:** not final. Reporting on the spring 2026 regulatory agenda says the final action date moved to **July 2027** (earlier target May 2026 missed), and that the item moved to the long-term list. Agenda dates are not binding, so check current status on reginfo.gov. Today's rule still applies. Building to the NPRM is cheap insurance: PulseLedger's controls below already assume MFA and encryption everywhere.

## Recognized security practices (Pub. L. 116-321)

A January 2021 amendment to HITECH (new §13412) tells HHS to consider whether an entity had **recognized security practices** in place for the previous **12 months** when setting penalties, audits or remedies. NIST CSF, the 405(d) HICP practices and similar programs count. It is a mitigating factor, not a safe harbor, and it must be **provable**: keep evidence ([controls_and_evidence.md](../14_Compliance_Program_and_Audit/controls_and_evidence.md)).

## PulseLedger's gaps against the table

| As-is fact | Standard it breaks |
| --- | --- |
| 2021 questionnaire as "risk analysis"; ScribeAssist, PulseWell, Chennai out of scope | §164.308(a)(1)(ii)(A) risk analysis |
| CloudTrail S3 data events off; app logs 30 days | §164.312(b) audit controls; §164.316(b)(2) |
| Legacy admin console has no MFA | §164.312(d) authentication |
| Unencrypted EBS volume of 2019–2021 claims | §164.312(a)(2)(iv) encryption (A, undocumented) |
| 4 coders share one Riverbend login | §164.312(a)(2)(i) unique user ID |
| No BAAs with LogPeak, speech-to-text vendor, staffing firm | §164.308(b)(1); §164.314(a) |
| Chennai VDI allows clipboard and printing | §164.310(b)–(d) workstation and media |
| No training records for Chennai contractors | §164.308(a)(5) |
| 2019 policies never reviewed | §164.316(b)(2)(iii) |

Continue with [risk_analysis_with_worked_example.md](./risk_analysis_with_worked_example.md) and [safeguards_mapped_to_aws.md](./safeguards_mapped_to_aws.md).

## Try this

Copy the gap table above into a sheet and add three columns: severity (H/M/L), the standard's R/A status, and the evidence you would show an OCR investigator. Pick the two gaps that are **Required** and unfixable by a policy alone. Write one sentence each on why. Then, for the unencrypted EBS volume, write the "addressable" decision as a documented note: either implement, or justify an alternative. Which is easier to defend?
