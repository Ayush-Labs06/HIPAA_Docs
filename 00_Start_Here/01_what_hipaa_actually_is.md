# What HIPAA actually is (the 20-minute mental model)

Most people think HIPAA means "health data is private". That is close, but too loose to work with. HIPAA is a set of US federal rules that bind **specific types of organizations** when they handle **specific types of health data**. Everything else in this repo follows from those two filters.

## The name is misleading

HIPAA is the Health Insurance Portability and Accountability Act of 1996 (Pub. L. 104-191). "Portability" meant keeping health insurance when you change jobs. Privacy was not the headline.

Title II, "Administrative Simplification", told HHS to standardize electronic health transactions (claims, eligibility, remittance). Congress knew standard electronic data would raise privacy and security risk, so it ordered privacy and security rules too. That is where the rules you will work with come from.

## The rules in one table

| Rule | Where | What it does |
| --- | --- | --- |
| Transactions and Code Sets | 45 CFR Part 162 | Standard formats (X12 837 claim, 835 remittance, 270/271 eligibility) and code sets (ICD-10, CPT) |
| Privacy Rule | 45 CFR Part 164, Subpart E | When PHI may be used or disclosed; patient rights |
| Security Rule | 45 CFR Part 164, Subpart C | Safeguards for electronic PHI (administrative, physical, technical) |
| Breach Notification Rule | 45 CFR Part 164, Subpart D | Who must tell whom when unsecured PHI is breached |
| Enforcement Rule | 45 CFR Part 160, Subparts C-E | Investigations, civil money penalties (CMPs), hearings |

```
45 CFR
├── Part 160  General Administrative Requirements
│   ├── Subpart A  General provisions (definitions: §160.103)
│   ├── Subpart B  Preemption of state law (§160.201-203)
│   ├── Subpart C  Compliance and investigations
│   ├── Subpart D  Civil money penalties
│   └── Subpart E  Hearings
├── Part 162  Administrative requirements (transactions, code sets, identifiers like NPI)
└── Part 164  Security and Privacy
    ├── Subpart A  General (§164.102-106)
    ├── Subpart B  [Reserved]
    ├── Subpart C  Security Standards for ePHI (§164.302-318)
    ├── Subpart D  Breach Notification (§164.400-414)
    └── Subpart E  Privacy of PHI (§164.500-534)
```

Cite as `45 CFR 164.308(a)(1)(ii)(A)` the first time, then `§164.308(a)(1)(ii)(A)`.

## Three laws layered on the 1996 Act

- **HITECH (2009).** Part of the American Recovery and Reinvestment Act (Pub. L. 111-5). It made business associates (BAs) directly liable, created the breach notification rule, raised penalties, and let state attorneys general sue.
- **The Omnibus Rule (2013).** Published 25 January 2013 (78 FR 5566), enforceable from 23 September 2013. It wrote HITECH into the regulations, expanded the BA definition to include subcontractors, and reshaped marketing, sale of PHI and breach assessment.
- **The 2024-2026 amendments.** See the "what's changing" box below.

## Who enforces

| Enforcer | Power | Authority |
| --- | --- | --- |
| HHS Office for Civil Rights (OCR) | Civil: investigations, resolution agreements, CMPs | 45 CFR Part 160 |
| Department of Justice | Criminal: knowingly obtaining or disclosing PHI | 42 U.S.C. 1320d-6 |
| State attorneys general | Civil suits for residents, under HITECH | 42 U.S.C. 1320d-5(d) |

## What HIPAA is not

- **Not all health data.** A fitness app a consumer downloads on their own is outside HIPAA. A hospital's record of the same step count is inside. Who holds the data matters, not what the data is. See [who_hipaa_applies_to.md](../01_Scope_and_Roles/who_hipaa_applies_to.md).
- **Not a certification.** No government body issues a "HIPAA certified" badge, and no course or website can grant one. Vendors who claim it are selling marketing. What exists instead: attestations, SOC 2 reports, and HITRUST assessments, which prove something about *controls*, not about legal compliance. A BAA plus a documented risk analysis is the real evidence.
- **No private right of action.** You cannot sue under HIPAA itself. Patients complain to OCR, and state-law claims (negligence, privacy torts) sometimes cite HIPAA as the standard of care.
- **Not a ban on sharing.** The Privacy Rule *permits* many disclosures (treatment, payment, operations). It is a permission system, not a lock.

## Where PulseLedger sits

PulseLedger Health is a **business associate** to its provider customers. It is not itself a covered entity (CE). So the Privacy Rule reaches it mostly through contracts (BAAs), while the Security Rule and Breach Notification Rule bind it directly. Details in [meet_pulseledger](./02_meet_pulseledger.md).

## What's changing (verify before you rely on any of this)

As of November 2026, based on my searches. Several sources are law-firm alerts; check the primary page.

| Item | Status |
| --- | --- |
| Security Rule overhaul (NPRM, 90 FR 898, 6 Jan 2025) | Proposed. Not final as of November 2026. Reported: the regulatory agenda moved final action from May 2026 to July 2027. Check current status. See [security_rule_explained.md](../06_Security_Rule/security_rule_explained.md) |
| 2024 reproductive-health Privacy Rule | Vacated nationwide by *Purl v. HHS* (N.D. Tex., 18 June 2025). Appeal reported closed in September 2025. Some notice-of-privacy-practices changes tied to Part 2 survive |
| 42 CFR Part 2 alignment rule (2024) | Compliance date 16 February 2026 has passed. OCR reported to enforce it. See [substance_use_records_part_2.md](../13_Special_Contexts/substance_use_records_part_2.md) |
| State consumer-health laws | Growing (Washington My Health My Data Act, others). They cover what HIPAA does not. See [health_apps_and_non_hipaa_data.md](../11_Tracking_Tech_and_Health_Apps/health_apps_and_non_hipaa_data.md) |

## How to read the rest of this repo

1. Work out the **role** (CE, BA, neither) in [01_Scope_and_Roles](../01_Scope_and_Roles/who_hipaa_applies_to.md).
2. Work out whether the **data** is PHI in [02_PHI_and_Data_Mapping](../02_PHI_and_Data_Mapping/what_is_phi.md).
3. Only then ask which rule applies and what to build.

## Try this

Classify each scenario as **HIPAA**, **not HIPAA** or **depends** (say what it depends on). Then check against the "where PulseLedger sits" logic.

1. A hospital emails a patient's lab result to another hospital for treatment.
2. A consumer logs sleep and medication data in PulseWell, which they downloaded themselves.
3. An employer's HR file holds a sick note from an employee.
4. PulseLedger coders in Pune read a Riverbend claim.
5. A health insurer's claims database is stored in AWS.
6. A life insurer reviews an applicant's medical history.
7. A school nurse keeps a student's vaccination record.
8. A gym's app syncs with a hospital's patient portal at the patient's request.

Write one sentence per answer naming the entity and the data. Keep it; you will redo it after module 01.
