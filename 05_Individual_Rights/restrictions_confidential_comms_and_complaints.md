# Restrictions, confidential communications and complaints (§164.522, §164.530(d))

Three rights about **control and voice**. A patient can ask the covered entity (CE) to limit how it uses PHI, to contact them in a private way, and to hear and answer their complaints. These are the individual rights that engineering must actually build, because they change what systems send, and to whom.

## Right to request restrictions (§164.522(a))

A person may ask the CE to restrict uses or disclosures of PHI for **TPO**, or to family and others involved in care (§164.510(b)). The default rule:

| Question | Answer |
| --- | --- |
| Must the CE agree? | **No.** It may say no (§164.522(a)(1)(ii)) |
| If it agrees? | It must honour the agreement, except in an emergency where the restricted PHI is needed for treatment |
| Can it end the agreement? | Yes, by telling the person (for later PHI), or by agreement of the person |
| Documentation? | Keep the agreed restriction in writing, 6 years (§164.530(j)) |

Restrictions do **not** bind disclosures to the individual, to HHS, or those required by law.

### The mandatory one: self-pay restriction

HITECH §13405(a), implemented in §164.522(a)(1)(vi), removes the CE's discretion in one case. The CE **must** agree if:

1. the disclosure is to a **health plan**, and
2. it is for **payment or health care operations** (not treatment), and is not required by law, and
3. the PHI relates **solely** to an item or service the individual **paid for in full, out of pocket**.

```
Patient pays cash in full for a visit, asks: "don't tell my insurer"
        │
        ▼
Provider flags the encounter: restriction = health-plan, self-pay
        │
        ├─▶ Claim to plan?        NO (for that encounter)
        ├─▶ Plan audit request?   NO (operations)
        └─▶ Another provider for treatment? YES, still allowed
```

Traps the CE and BA must handle: bundled care (a later service that cannot be billed without revealing the first); a referral that carries the restricted diagnosis; and a patient who later cannot pay and withdraws the restriction.

### PulseLedger engineering: the self-pay flag

LedgerRCM builds and submits the 837 claim. If the flag does not propagate, PulseLedger itself discloses the restricted service to the plan.

| Control point | What to build |
| --- | --- |
| Encounter record | `restriction_type='self_pay_plan'`, `set_by` (CE user), `set_on`, `scope_encounter_ids` |
| Eligibility (270/271) | Do not check or record restricted encounters against the plan |
| Claim scrubber | **Hard stop**: if `restriction_type` is present, block 837 build for that encounter and queue for CE review |
| Clearinghouse feed | Exclude flagged encounters; test with a synthetic restricted claim |
| Denials / 835 posting | If a plan 835 arrives anyway, quarantine it and alert the CE; do not auto-post |
| PulseConnect statements | Send only to the patient, never to the plan or guarantor on the plan |
| Evidence | Monthly report of flagged encounters and "claims blocked" counts |

The CE owns the decision to honour the request. PulseLedger owns making the system do it. Contract language goes in the BAA: [../08_Business_Associate_Agreements_and_Vendors/baa_clause_by_clause_review.md](../08_Business_Associate_Agreements_and_Vendors/baa_clause_by_clause_review.md).

## Confidential communications (§164.522(b))

A person may ask to receive PHI **by alternative means or at an alternative location**: a different mailing address, a work phone, no voicemail, email instead of letters.

| Entity | Must it accommodate? |
| --- | --- |
| **Provider** | Yes, if the request is **reasonable**. It may not ask why |
| **Health plan** | Yes, if the person says disclosure could **endanger** them. It may require that statement |

The CE may require the request in writing, and may require information on how payment will be handled. It cannot require an explanation from a provider patient.

This matters for people living with an abusive partner, teens seeking sensitive care, and anyone who simply does not want a text on a shared phone. See [../13_Special_Contexts/minors_representatives_and_sensitive_care.md](../13_Special_Contexts/minors_representatives_and_sensitive_care.md).

### PulseConnect: "do not text my home phone"

PulseConnect sends appointment reminders by SMS and email. An alternate-contact request must reach every sender.

```
CE staff records request ──▶ patient_contact_prefs (per CE, per patient)
                                   │
        ┌──────────────────────────┼──────────────────────────┐
        ▼                          ▼                          ▼
  SMS gateway job           Email job                Statement print job
  checks prefs before       checks prefs             uses alt address
  send                      before send              (never the old one)
```

Failure modes: a cached recipient list; a second system (the SMS vendor's own opt-out list) that does not know the preference; a statement already in the print queue; and a reminder that includes the clinic name or visit type on a shared phone. A confidential-comms flag should also trim message content to "You have an appointment. Call us."

## Complaints (§164.530(d))

- **To the CE:** the CE must have a process for complaints about its privacy practices, name a contact person, and document each complaint and its outcome for 6 years.
- **To HHS:** a person may file with OCR, normally within **180 days** of when they knew of the act, which OCR can extend for good cause (§160.306(b)(3)). The CE's NPP must say how: [../04_Notice_of_Privacy_Practices/notice_of_privacy_practices.md](../04_Notice_of_Privacy_Practices/notice_of_privacy_practices.md).
- **BAs:** a patient may complain directly to PulseLedger. Log it, route it to the CE within the BAA clock, and do not argue the merits.

## Non-retaliation and no waiver

| Rule | Meaning |
| --- | --- |
| **§164.530(g)** | No retaliation against anyone for filing a complaint, helping OCR, or opposing an unlawful act in good faith |
| **§164.530(h)** | A CE may not require a person to waive their rights (including the right to complain to HHS) as a condition of treatment, payment, enrollment or eligibility |

PulseLedger angle: a staff member who reports the INC-2026-035 snooping must be protected the same way. See [../01_Scope_and_Roles/privacy_and_security_officers.md](../01_Scope_and_Roles/privacy_and_security_officers.md).

## Rights-handling register

| Request | Who decides | PulseLedger action | Clock |
| --- | --- | --- | --- |
| Restriction (general) | CE | Log, route; apply if the CE agrees | CE policy |
| Self-pay to plan | CE, mandatory | Flag and block downstream | Before next claim |
| Confidential communication | CE | Update contact preferences in all sender jobs | Before next send |
| Complaint | CE privacy officer | Log, route, preserve evidence | BAA notice clock |

## Try this

Design the data fields and logic for "do not text my home phone". Give: (1) a `patient_contact_prefs` table (columns, types, who sets each), (2) pseudocode for the send-time check shared by the SMS and email jobs, (3) the rule that applies when no safe alternative exists (fail closed: send nothing and alert the CE), (4) one automated test with a synthetic patient, and (5) the evidence you would show an auditor.
