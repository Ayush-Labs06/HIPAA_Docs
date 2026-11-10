# Uses and disclosures: the TPO backbone (§164.502, §164.506)

The Privacy Rule starts from a "no". A covered entity (CE) or business associate (BA) may not use or disclose protected health information (PHI) unless the Rule **permits** or **requires** it (45 CFR 164.502(a)). Everything else in module 03 is a door cut into that wall. The widest door is **TPO**: treatment, payment and health care operations.

## Required vs permitted

Only two disclosures are **required** (§164.502(a)(2)):

1. **To the individual** when they ask for access (§164.524) or an accounting (§164.528). See [right_of_access.md](../05_Individual_Rights/right_of_access.md).
2. **To HHS** (OCR) when it investigates or checks compliance.

Everything else is *permitted*, not required. "HIPAA allows it" never means "you must do it". Your BAA or state law may still say no.

The permitted list in §164.502(a)(1): to the individual; for TPO; incident to another permitted use; with a valid authorization; under the §164.510 "opportunity to agree or object" rules; under the §164.512 public-interest list; and as a limited data set.

## TPO in plain words

| Purpose | Meaning (§164.501) | Examples |
| --- | --- | --- |
| **Treatment** | Providing, coordinating or managing care, including referrals and consults between providers | Hospital sends a discharge summary to the patient's GP |
| **Payment** | Getting paid or paying: billing, claims management, eligibility checks, coverage determinations, collections, utilization review | A practice sends an 837 claim to a payer; a payer posts an 835 remittance |
| **Health care operations** | The CE's own business functions: quality assessment, training, credentialing, audits, legal and compliance, business planning, customer service | A hospital reviews coding accuracy; an internal compliance audit |

Key points:

- **No authorization is needed for TPO.** A "consent to treat" form is not a HIPAA requirement. Providers *may* ask for consent (§164.506(b)), but a HIPAA authorization is a different, stricter thing.
- **Treatment is the widest.** A provider may disclose PHI for another provider's treatment, and the minimum necessary standard does not apply to treatment disclosures ([minimum_necessary.md](./minimum_necessary.md)).
- **Payment** may also be disclosed to another CE or provider for *their* payment activities (§164.506(c)(3)). Example: a hospital gives a physician group the data it needs to bill.
- **Operations** disclosures to *another* CE are narrower (§164.506(c)(4)): both must have a relationship with the patient, and the purpose must be quality, competency review, or fraud and abuse detection.
- **Not TPO:** marketing, sale of PHI, a vendor's own product analytics, and training an AI model for the vendor's benefit. These need an authorization or a de-identification route ([authorizations_and_special_permissions.md](./authorizations_and_special_permissions.md)).

## The permissions ladder

Work down the ladder. Stop at the first rung that fits.

```
Is the person asking the individual themselves (or their personal representative)?
   yes -> permitted/required (access, accounting)           -> stop
   no  |
        v
Is it for Treatment, Payment or Health care Operations of the CE?
   yes -> permitted, apply minimum necessary (not for treatment)
   no  |
        v
Is it incident to a permitted use, with safeguards in place?
   yes -> permitted (incidental, §164.502(a)(1)(iii))
   no  |
        v
Is it to family/friends involved in care, or a facility directory?
   yes -> §164.510: agree/object, or professional judgment
   no  |
        v
Does one of the 12 public-interest rules in §164.512 fit?
   yes -> permitted, follow that paragraph's conditions
   no  |
        v
Is it a limited data set under a data use agreement?
   yes -> permitted (§164.514(e))
   no  |
        v
Do you have a valid authorization (§164.508)?
   yes -> permitted, only as the authorization says
   no  -> DO NOT disclose
```

Special categories sit above the ladder: **psychotherapy notes** (authorization nearly always), **42 CFR Part 2** SUD records (their own consent rules, see [substance_use_records_part_2.md](../13_Special_Contexts/substance_use_records_part_2.md)), and state laws that are stricter.

## Incidental disclosures

A hushed conversation at a registration desk that someone overhears is not a violation, **if** you used reasonable safeguards and minimum necessary (§164.502(a)(1)(iii)). Sign-in sheets, privacy screens and low voices are the usual examples. An incidental disclosure is not a breach by itself, but a pattern of weak safeguards is a Security Rule and Privacy Rule problem.

## Family and friends (§164.510(b))

- If the patient is present and able, **ask or give a chance to object**.
- If they are absent or incapacitated, use professional judgment about their best interest and share only what the person involved in care needs (for example, letting a relative pick up a prescription).
- Notifying family about location, condition or death is allowed under the same logic.
- Note the limits: you may not hand a relative a full chart because they "are family".

## Where PulseLedger sits

PulseLedger Health is a BA ([meet_pulseledger.md](../00_Start_Here/02_meet_pulseledger.md)). A BA gets **no permissions of its own** beyond what the BAA gives it (§164.502(a)(3), §164.504(e)). Three consequences:

- **LedgerRCM is "payment" done on the CE's behalf.** Eligibility checks (270/271), coding, claim scrubbing, 837 submission and 835 posting are payment activities. PulseLedger may do them because the customer's BAA says so.
- **A BA may use PHI for its own management and administration** (§164.504(e)(4)) only if the BAA allows it. Many customer BAAs allow it narrowly and forbid secondary use.
- **Product analytics, model training and benchmarking are not TPO for the customer.** They need an explicit BAA term or properly de-identified data ([de_identification_and_limited_data_sets.md](../02_PHI_and_Data_Mapping/de_identification_and_limited_data_sets.md)).

A filled example:

| PulseLedger use | Permitted? | Why |
| --- | --- | --- |
| Offshore coder assigns ICD-10/CPT to Riverbend encounters | Yes | Payment on behalf of the CE; BAA covers it; minimum necessary applies |
| Sending an 837 through the clearinghouse | Yes | Payment; clearinghouse is a subcontractor BA with a BAA |
| Using Lakeshore claims to build a cross-customer denial benchmark sold to payers | No (as-is) | Not TPO for Lakeshore; looks like sale or secondary use. Needs de-identification and a BAA term |
| Pasting an error extract into a chat tool with no BAA | No | Disclosure to a vendor with no BAA. See INC-2026-027 in [is_it_a_breach.md](../07_Breaches_and_Incidents/is_it_a_breach.md) |

## Try this

Classify each PulseLedger data use as **permitted (TPO)**, **permitted (other rung)**, **needs authorization**, or **not permitted as-is**. Give the rung and one sentence of reasoning.

1. A coder in Chennai checks a patient's insurance eligibility.
2. PulseConnect texts an appointment reminder for a Riverbend clinic.
3. Engineering copies 2,000 production claims into `pl-dev` to debug a scrubber rule.
4. Support sends a Lakeshore patient's balance to the patient's mother, who called asking.
5. ScribeAssist keeps customer dictation to fine-tune PulseLedger's own model.
6. A police officer phones Nashville asking whether a named person is a Riverbend patient.
7. PulseConnect sends a pharma-sponsored "ask your doctor about..." message.
8. The finance team shares a de-identified (Safe Harbor) denial-rate table with an investor.
9. A Riverbend billing manager asks PulseLedger for a patient's 835 to resolve an underpayment.
10. A staffing-firm contractor reviews claim samples on a personal laptop.
