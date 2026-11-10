# Minors, personal representatives and sensitive care

HIPAA's default is simple: the patient controls their own PHI. Real life is not. Children cannot always consent, adults sometimes act for others, and some kinds of care carry extra legal weight. This file is about *who counts as the individual* and what extra layers sit on top.

A reminder before we start. PulseLedger is a business associate (BA). The covered entity (CE) decides who is a personal representative and what is shared. PulseLedger builds the switches (contacts, flags, queues) and follows the customer's instructions.

## Personal representatives (§164.502(g))

A **personal representative** is a person who has legal authority to act for an individual in health care decisions. HIPAA treats them as the individual for the matters within that authority.

| Situation | Personal representative? | Basis |
| --- | --- | --- |
| Parent of an unemancipated minor | Usually yes | §164.502(g)(3) |
| Adult with a healthcare power of attorney | Yes, for the decisions it covers | State law |
| Court-appointed guardian | Yes, within the order | State law |
| Executor or administrator of a deceased person's estate | Yes | §164.502(g)(4) |
| Spouse or adult child with no legal authority | No (but §164.510(b) may allow limited sharing) | [../03_Privacy_Rule/uses_and_disclosures_tpo.md](../03_Privacy_Rule/uses_and_disclosures_tpo.md) |

## Minors: when the parent is *not* the representative

The parent is **not** the minor's personal representative (§164.502(g)(3)(i)) when:

1. The minor consents to the care, and state law does not require the parent's consent. Examples in many states: contraception, STI testing, some mental health or SUD care (ages and rules vary by state; check current state law).
2. A court (or someone authorised by a court) directs the care.
3. A parent agrees to a confidentiality term between the clinician and the minor.

Then the **minor** controls that PHI. What the parent can see is set by state law. Where state law is silent, the provider has discretion under §164.502(g)(3)(ii)(C), using professional judgment.

**Abuse and endangerment (§164.502(g)(5)).** A CE may decline to treat someone as the representative if it reasonably believes the person abused or neglected the individual, or that treating them as representative could endanger the individual. This needs a professional judgment, written down.

## Sensitive categories

| Category | Extra rule | Practical effect |
| --- | --- | --- |
| Psychotherapy notes | Separate from the mental health record; authorization needed for most uses (§164.508(a)(2)) | LedgerRCM should never need them. Reject if received. See [../03_Privacy_Rule/authorizations_and_special_permissions.md](../03_Privacy_Rule/authorizations_and_special_permissions.md) |
| Mental health records | Ordinary PHI under HIPAA; many states add stricter rules (check state) | Flag for state-law review per customer |
| SUD treatment | 42 CFR Part 2 | [./substance_use_records_part_2.md](./substance_use_records_part_2.md) |
| HIV/AIDS, STI | Mostly state law, often stricter than HIPAA | Claim lines can reveal status; mask in reminders |
| Genetic information | HIPAA counts it as health information; health plans may not use or disclose it for underwriting (§164.502(a)(5)(i)); GINA Title I adds limits on health insurers | Matters for Summit Valley as a plan customer |
| Reproductive health | See below | Highly volatile |

## Reproductive health care

In April 2024 HHS finalized a HIPAA amendment that limited disclosures of PHI for investigating or punishing lawful reproductive health care, and required an attestation for certain requests. A federal court in Texas (*Purl v. HHS*, N.D. Tex.) **vacated** nearly all of it nationwide on 18 June 2025. As of November 2026:

- The special reproductive-health prohibition and the attestation requirement are not in force. Ordinary HIPAA rules apply, including §164.512(f) (law enforcement) and §164.512(e) (court orders and subpoenas).
- The reproductive-health changes to the NPP were severed. The Part 2 NPP changes remain.
- Whether an appeal or fresh rulemaking is pending: **check current status**.
- Some **state shield laws** and state health privacy laws still protect this data and limit cooperation with out-of-state investigations. These vary. Treat any request touching reproductive care as a legal-escalation event, not a support ticket. Route it through the subpoena workflow in [../03_Privacy_Rule/public_interest_disclosures.md](../03_Privacy_Rule/public_interest_disclosures.md).

Also note that an information-blocking exception for reproductive care access was added in 2024 (45 CFR 171.206); its current status is volatile (see [../15_Related_Laws/information_blocking_and_interoperability.md](../15_Related_Laws/information_blocking_and_interoperability.md)).

## PulseConnect: reminders to a teen's parent

PulseConnect sends SMS and email reminders. The risky case: a 16-year-old books a confidential visit; the reminder goes to the parent's phone on the account.

Design for the customer's configuration:

```
 patient record
   ├─ patient_dob → minor? (state age of majority from customer config)
   ├─ contact_roles[]
   │     ├─ role: patient | guardian | alternate
   │     ├─ channel: sms | email | portal
   │     └─ allowed_content: full | minimal | none
   ├─ sensitive_visit_flag (set by the customer from the scheduling system)
   └─ confidential_comm_request (§164.522(b)) → overrides default contact
```

Rules to build:

1. Default reminder content is **minimal**: date, time, location, no clinic name if the clinic name reveals the service (a reproductive or SUD clinic name is itself sensitive).
2. When `sensitive_visit_flag=true`, send only to the contact the *minor* chose, or send nothing.
3. A confidential-communications request wins over the default. See [../05_Individual_Rights/restrictions_confidential_comms_and_complaints.md](../05_Individual_Rights/restrictions_confidential_comms_and_complaints.md).
4. The customer sets the rules per state; PulseLedger enforces and logs which rule fired.
5. Statements and EOBs have the same problem: an explanation of benefits mailed to a policyholder can reveal a dependant's care.

## Try this

Decide who receives the appointment-reminder SMS in each case. State the rule and what you would ask the customer.

| # | Scenario |
| --- | --- |
| 1 | A 9-year-old's well-child visit; both parents on file |
| 2 | A 16-year-old, state allows minors to consent to STI testing; visit flagged sensitive; parent's number on the account |
| 3 | An adult patient with a court-appointed guardian; patient also supplied their own number |
| 4 | An adult asks that reminders go to a work-friendly alternate phone and never the shared home number |
