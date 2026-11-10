# Employers, group health plans and HIPAA

Many people think HIPAA stops an HR manager from asking about health. It does not. HIPAA binds the *health plan*, not the *employer*. Getting this line right saves you from both over-claiming ("HIPAA says we can't ask") and under-protecting (leaving plan data on a shared HR drive).

## Two hats, two sets of rules

An employer wears two hats:

| Hat | Is it a covered entity (CE)? | Health info it holds | Law that applies |
| --- | --- | --- | --- |
| **Employer** (hiring, sick leave, fitness for duty, accommodations) | No | Employment records | ADA, FMLA, GINA Title II, state law |
| **Plan sponsor** of a group health plan | The *plan* is a CE; the employer is the sponsor | PHI the plan gives the sponsor | HIPAA Privacy, Security and Breach rules |

**Employment records are not PHI.** The definition of PHI in §160.103 expressly excludes employment records held by a CE in its role as employer. A sick note in an HR file is not PHI, even if the employer is a hospital.

A **group health plan** (GHP) is a CE (§160.103) if it has 50 or more participants or is administered by someone other than the employer that set it up. Small self-administered plans (under 50) are outside the definition.

## Fully insured vs self-insured

| | Fully insured | Self-insured (self-funded) |
| --- | --- | --- |
| Who bears the claims risk | The insurance carrier | The employer |
| CE status | The carrier is a CE (health insurance issuer). The plan is also a CE but may do very little | The plan is a CE and does the work |
| Employer's access to PHI | Often only summary health information and enrollment data | Often needs detailed claims for administration |
| Employer's HIPAA load | Light, if it never touches PHI (§164.530(k)) | Full: policies, notice, Security Rule, BAAs with the TPA |
| Third-party administrator | Not usually | BA of the plan |

**PulseLedger's position.** The group health plan for its 80 US employees is **fully insured** through a carrier. If PulseLedger HR receives only enrolment information and summary data, its plan duties are light. If an HR person starts asking the carrier for individual claim details "to help an employee", that moves the company into plan-sponsor duties. Stop and use the firewall below.

## Plan sponsor rules and firewalls (§164.504(f))

A plan may share PHI with the sponsor only for plan administration, and only if the plan documents are amended and the sponsor certifies it will:

1. Not use PHI for employment decisions or other benefits.
2. Not disclose it except as the plan permits.
3. Report improper uses to the plan.
4. Make PHI available for individual rights (access, amendment, accounting).
5. Return or destroy PHI when no longer needed, if feasible.
6. Keep an **adequate firewall**: name the employees (by role) allowed to see PHI, limit them to plan administration, and set a sanction process (§164.504(f)(2)(iii)).

A firewall is a list plus a technical block. In practice:

```
HR Benefits team (named roles: Benefits Manager, Payroll lead)
   └─ can read: enrolment, premium, summary health info
   └─ can read PHI only in the carrier portal, under named logins
HR Business Partners / line managers
   └─ cannot see plan data (no access group, no shared folder)
Security: access group `grp-gh-plan`, reviewed quarterly,
          audit of reads in the benefits system
```

## Wellness programs

A wellness programme can sit in two places, and the law differs:

- **Offered through the group health plan.** HIPAA applies to the data. The HIPAA/ACA nondiscrimination rules limit rewards tied to health factors (the reward cap and reasonable-alternative rules; see the current Departments' rules).
- **Offered by the employer outside the plan.** Not HIPAA. Data is employment data. The ADA (voluntary participation, confidentiality) and GINA (family medical history, genetic data) apply, and the EEOC rules have been in litigation, so check current status.

Either way, never use wellness data in hiring, promotion or discipline.

## COVID-era lessons

A common mistake in 2020–2022 was "you cannot ask my vaccination status; HIPAA". HHS OCR said plainly that HIPAA does not stop an employer from asking employees about vaccination status, because HIPAA covers CEs and BAs, not employers in that role. Other laws may still limit it: ADA confidentiality (keep it in a separate medical file), state laws, and anti-discrimination rules. The lesson for GRC: when someone says "that's a HIPAA issue", first ask *which entity is holding the data in which role*.

## PulseLedger as an employer

| Data | HIPAA? | What applies |
| --- | --- | --- |
| US staff: insurance enrolment from the carrier | Plan-related; sponsor duties if PHI received | Firewall, minimal access |
| US staff: sick notes, FMLA, accommodation requests | No (employment records) | ADA/FMLA confidentiality; separate medical file; HR-only |
| India staff (Pune, Chennai): sick leave, medical insurance, health checks | No | India law: DPDP Act 2023 applies from 13 May 2027 to Indian data; employment-related processing has a legitimate-use ground (check the Act's section 7); until then the IT Act s.43A / SPDI Rules question is open (check current status) |
| Contractors via the staffing firm | No | The staffing firm is the employer; contract terms |
| Employee who is also a patient of a customer | No special rule | If PulseLedger staff see their *own* or a colleague's claims in LedgerRCM, that is a workforce access issue, not an employment record |

That last row matters: coder access to a colleague's claim history is a real snooping risk. Covered in [../06_Security_Rule/risk_analysis_with_worked_example.md](../06_Security_Rule/risk_analysis_with_worked_example.md) and [../14_Compliance_Program_and_Audit/controls_and_evidence.md](../14_Compliance_Program_and_Audit/controls_and_evidence.md).

## Real enforcement

OCR has no wellness-programme enforcement history to cite here. For employer-side health privacy, enforcement comes from the EEOC and state agencies, not OCR. Do not invent a HIPAA case for an HR scenario; most are not HIPAA at all.

## Try this

Classify each HR scenario as **HIPAA / not HIPAA / depends**, and name the law or rule that applies.

| # | Scenario |
| --- | --- |
| 1 | A manager asks a US employee why they were absent and keeps the sick note in the HR file |
| 2 | The HR benefits lead asks the insurance carrier for one employee's detailed claim to "check a billing error" |
| 3 | A self-funded plan sends claims data to its TPA |
| 4 | An India-based employee's health-check report sits on the Pune HR share |
| 5 | The company asks all staff to share COVID vaccination status |
| 6 | A voluntary step-count challenge run by the employer through an app, with no plan involvement |
