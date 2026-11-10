# Amendment and accounting of disclosures (§164.526, §164.528)

Two smaller rights, often mixed up. **Amendment** lets a person ask to correct their record. **Accounting** lets a person ask "who did you share my PHI with, outside the normal routine?" Both belong to the covered entity (CE). A business associate (BA) supplies the data and the logs.

## Amendment (§164.526)

A person may ask the CE to amend PHI **in the designated record set** (see [right_of_access.md](right_of_access.md)) for as long as the CE keeps it.

```
Request received (writing may be required if the NPP says so)
   │
   ▼  Day 0-60: CE must act
Accept ──▶ amend, tell the person, tell people they named + BAs who
   │       hold the record and may rely on it (§164.526(c))
Deny   ──▶ written denial in plain language
   │
   ▼  Day 30 extension: once, with written reason and date
```

- **Deadline:** act within **60 days**, one extension of up to **30 days** (§164.526(b)(2)).
- **Accepting** means more than editing one screen. The CE must make the change (or link the amendment) wherever the record lives, and tell others who need to know.
- **Four grounds to deny** (§164.526(a)(2)): the CE did not create the PHI (unless the originator is no longer available); it is not in the DRS; it is not available for inspection under §164.524; or it is already accurate and complete.
- **Denial letter must include:** the basis, the right to file a **statement of disagreement**, the right to ask that the request and denial go with future disclosures, and how to complain to the CE and HHS.
- **After denial:** the person files a short disagreement; the CE may write a rebuttal and must send a copy. The request, denial, disagreement and rebuttal travel with the record in later disclosures.
- **Coding note:** in RCM, a patient disputing a diagnosis code is often really disputing the clinical note. The coder at a BA cannot change clinical documentation. Route it to the provider.

### PulseLedger angle

A BA must make PHI available for amendment and incorporate amendments the CE directs (§164.504(e)(2)(ii)(F)). In LedgerRCM that means a **customer-initiated correction path**: the CE's user changes a field; the change is versioned (old value kept), logged, and pushed to downstream copies (clearinghouse resubmission, PulseConnect statements). Do not let Pune or Chennai coders overwrite clinical fields on a patient's word.

## Accounting of disclosures (§164.528)

A person may ask for a list of disclosures of their PHI in the **6 years** before the request (or a shorter period if they ask).

### What is excluded (§164.528(a)(1))

| Excluded from the accounting | Example |
| --- | --- |
| Treatment, payment, health care operations (TPO) | Sending a claim through the clearinghouse |
| To the individual, or to a personal representative | Statement mailed to the patient |
| Incident to an otherwise permitted disclosure | A name overheard at a counter |
| Under a valid authorization | Records to a life insurer the patient chose |
| Facility directory, or to persons involved in care | A hospital room lookup |
| National security or intelligence; custodial settings | Lawful requests under §164.512(k) |
| As part of a limited data set | A research LDS under a DUA |

What is **left**, and must be accounted for: disclosures required by law, public health, abuse reports, health oversight, judicial and administrative proceedings, law enforcement, research under a waiver, decedents, serious threat, and **impermissible disclosures, including breaches**. See [../03_Privacy_Rule/public_interest_disclosures.md](../03_Privacy_Rule/public_interest_disclosures.md).

So INC-2026-019 (the 835 batch emailed to the wrong practice) belongs in the accounting of each of the **412** people, even though nobody asked for it.

### Content and timing

- Each entry: **date**, **recipient name** (and address if known), **brief description of the PHI**, and **purpose or a copy of the request** (§164.528(b)(2)).
- **Multiple disclosures** to the same recipient for one purpose may be summarised, with first and last dates and frequency (§164.528(b)(3)). Research disclosures involving 50+ people have a simplified form (§164.528(b)(4)).
- **Deadline:** 60 days, one 30-day extension. **Fee:** the first accounting in any 12 months is free; after that a reasonable cost-based fee, with notice first.
- **Suspension:** a health oversight agency or law enforcement official can ask the CE to suspend the accounting for a period (§164.528(a)(2)).
- **Retain** the accounting and who handled it for 6 years (§164.528(d)).

### The HITECH change that never landed

HITECH §13405(c) was meant to remove the TPO exclusion for disclosures through an EHR. HHS proposed a rule on 31 May 2011 (a TPO-inclusive "access report" and a narrower accounting). I found no final rule and no formal withdrawal. **Check current status** before saying it is dead. Today the 2000-era §164.528 above is the rule in force.

## What a BA logs to support accounting

The BAA must require the BA to document disclosures and give the CE the information (§164.504(e)(2)(ii)(G)). The BA cannot reconstruct this from memory, so build a log.

### Disclosure-log schema

| Field | Example | Note |
| --- | --- | --- |
| `disclosure_id` | `DL-2026-000412` | Unique |
| `occurred_at` | `2026-05-12T09:40:00Z` | UTC |
| `ce_id` | `riverbend` | Whose patient |
| `patient_ref` | `MRN-77120` | Tokenised, one row per patient |
| `recipient_name` | "Practice B" | Named person or entity |
| `recipient_address` | if known | |
| `phi_description` | "835 remittance batch, 2 months" | Brief |
| `purpose` | "Misdirected email (impermissible)" | Or request copy |
| `basis` | `impermissible` / `required_by_law` / `public_health` | Drives inclusion |
| `source_event` | ticket or incident ID | `INC-2026-019` |
| `logged_by` | user ID | |
| `ttl` | 6 years from the date | Do not default to the 30-day app-log window |

PulseLedger's as-is app audit logs last **30 days**, so six-year accounting is impossible from them. Keep the disclosure log as a separate, immutable record (S3 with Object Lock in `pl-security`). Log retention design: [../06_Security_Rule/safeguards_mapped_to_aws.md](../06_Security_Rule/safeguards_mapped_to_aws.md).

### Sample entries

| Date | Recipient | PHI | Purpose |
| --- | --- | --- | --- |
| 2026-05-12 | Practice B (a physician practice) | 835 remittance data | Sent by mistake; deletion confirmed in writing 2026-05-13 |
| 2026-06-20 to 2026-06-21 | Unknown (public bucket, 31 hours) | Dictation audio | Exposure in INC-2026-024; access cannot be ruled out |
| 2026-07-30 | State health department | Claim fields | Required by law, case-specific |

## Try this

Using the sample entries above plus these two extra rows, produce the accounting for one patient, "J. Rao", whose data appears in INC-2026-019 and in a health-department disclosure on 2026-07-30; a TPO claim to the clearinghouse on 2026-07-05 must be left out. Write the response letter, show the 60-day deadline if the request arrives on 2026-09-01, and say which log fields you would have missed if you had used only the 30-day app logs.
