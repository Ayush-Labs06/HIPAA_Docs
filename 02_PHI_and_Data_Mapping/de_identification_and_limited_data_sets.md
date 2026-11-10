# De-identification and limited data sets (§164.514)

HIPAA has an exit door: data that is properly **de-identified** is no longer PHI, so the Privacy, Security and Breach rules stop applying to it. Between "full PHI" and "de-identified" sits a middle option, the **limited data set (LDS)**. Both are tightly defined. "We removed the names" is not one of them.

## The spectrum

```
Full PHI ──▶ Limited data set ──▶ De-identified data
 (all 18       (direct identifiers     (Safe Harbor or Expert
 identifiers)   removed; dates, city,    Determination met;
                state, ZIP kept;         not PHI at all)
                needs a DUA)
 Still PHI      Still PHI               Not PHI
```

Who may do it: a covered entity (CE) may de-identify its own PHI. A business associate (BA) may do so **only if its BAA allows it** (§164.502(d), §164.504(e)). De-identifying is itself a *use* of PHI. PulseLedger therefore needs a written right in each customer BAA before it can build analytics or model-evaluation sets from customer data. See [baa_clause_by_clause_review.md](../08_Business_Associate_Agreements_and_Vendors/baa_clause_by_clause_review.md).

## Method 1: Safe Harbor (§164.514(b)(2))

Remove all **18 identifiers** of the individual **and** of relatives, employers and household members (full list in [what_is_phi.md](what_is_phi.md)), **and** have no **actual knowledge** that what remains could identify the person.

Where people slip:

| Element | Safe Harbor rule |
| --- | --- |
| **Dates** | Keep **year only**. Remove month and day of birth, admission, discharge, service and death. Ages over 89 and all dates indicating such an age are grouped into "90 or older" |
| **Geography** | Remove street, city, county, precinct, ZIP. You may keep the first **3 ZIP digits** only if all ZIP codes with that prefix together hold **more than 20,000 people**. Otherwise set them to `000`. Use current Census data to decide; check the source and date of your list |
| **Free text** | Names, MRNs and dates hide in notes. Each must be found and removed |
| **Voice and images** | Voice prints and full-face or comparable images are identifiers. A dictation recording cannot meet Safe Harbor as audio |
| **Codes** | A unique code you derive from the person (a hash of the MRN, say) is itself a "unique identifying code" (identifier R) |
| **Date shifting** | Shifting every date by a random offset still leaves date elements. OCR's guidance does not treat that as Safe Harbor. Use Expert Determination if you need intervals |
| **Actual knowledge** | If your analyst knows "patient 4410 is the mayor", the data is not de-identified |

Safe Harbor is simple to audit and costly in utility. You lose dates and fine geography.

## Method 2: Expert Determination (§164.514(b)(1))

A person with appropriate knowledge and experience in statistical and scientific methods for rendering information not individually identifiable determines that the **risk of identification is very small** by an anticipated recipient, and documents the methods and results.

OCR's *Guidance Regarding Methods for De-identification of Protected Health Information in Accordance with the HIPAA Privacy Rule* (26 November 2012) explains both methods. It makes these points:

- There is no required degree or certification for the expert; qualifications are judged by experience.
- "Very small" is **not** a fixed number. The expert reasons about the data, the recipient and what else the recipient could link it to.
- The determination is **specific to a recipient and context**. Releasing the same dataset publicly is a different risk than sharing it under contract with one partner.
- There is no mandatory expiry, but linkable outside data changes. Set a review period.
- Contract terms (no re-identification attempts, no linkage) can be part of what the expert relies on.

Use Expert Determination when you need dates, fine geography or rich free text that Safe Harbor would strip.

## Re-identification codes (§164.514(c))

A CE may assign a code to let *it* re-link de-identified records later, if:

1. the code is **not derived from** information about the individual (a random number, not a hash of the MRN), and
2. it is not otherwise capable of being translated to identify the person, **and**
3. the CE does not use or disclose the code or the mapping for any other purpose, and does not disclose the mechanism for re-identification.

If a de-identified record is later re-identified, it becomes PHI again.

## Limited data sets (§164.514(e))

An LDS removes **direct identifiers** (names, street addresses, phone and fax, email, SSN, MRN, health plan and account numbers, certificate and licence numbers, vehicle and device identifiers, URLs, IP addresses, biometric identifiers, full-face photos) but may keep **dates, city or town, state, and 5-digit ZIP** and ages.

- An LDS is **still PHI**. Breach rules apply.
- It may be used or disclosed only for **research, public health or health care operations**.
- The recipient must sign a **data use agreement (DUA)** with the CE (§164.514(e)(4)). The DUA must establish permitted uses, who may use or receive the data, and that the recipient will: not use or further disclose beyond the agreement or law, use appropriate safeguards, report unauthorized use, make agents agree to the same limits, and **not identify or contact the individuals**.
- Minimum necessary applies (see [minimum_necessary.md](../03_Privacy_Rule/minimum_necessary.md)).

| | Limited data set | De-identified |
| --- | --- | --- |
| Is it PHI? | Yes | No |
| Dates and ZIP-5 allowed | Yes | No (year; ZIP-3 if large enough) |
| Paperwork | DUA required | None required by HIPAA (use contract terms anyway) |
| Purposes | Research, public health, operations only | Any |
| Breach notification | Applies | Does not apply |

## Pseudonymization is not de-identification

Replacing `patient_name` with `P-83712` is **pseudonymization**. The person can still be singled out and, via a key or linkable fields, re-identified. Unless the whole dataset also passes Safe Harbor or Expert Determination, it is **PHI**. This differs from GDPR thinking, where pseudonymised data is also still personal data. Under HIPAA, "tokenized but linkable" is still PHI. Tokenization is a great *control* (it shrinks exposure in dev and logs), but it does not take data out of scope.

## Worked example: a ScribeAssist evaluation dataset

Goal: Sneha Joshi's team wants 5,000 SOAP notes to test a new LLM prompt. Source: ScribeAssist notes drafted from real customer encounters.

```
Production notes ──▶ [1] BAA check ──▶ [2] strip structured fields
      (PHI)             allowed?          ──▶ [3] scrub free text ──▶ [4] audio dropped
                                                                        │
         [7] release ◀── [6] written ◀── [5] sample manual review ◀─────┘
        (pl-sandbox)      sign-off
```

1. **BAA check.** Do the customer BAAs let PulseLedger de-identify for product evaluation? Where they do not, stop; use only synthetic data. (See [ai_with_phi.md](../12_AI_and_Health_Data/ai_with_phi.md).)
2. **Structured fields.** Drop name, MRN, member ID, phone, address. Reduce DOB to year; ages over 89 become `90+`. Service date to year. ZIP to `000`-or-ZIP3 by the 20,000 rule.
3. **Free text.** Run a clinical de-identification tool, then regex for MRN and phone patterns. Remove clinician names too if they could point to the patient (a note signed by the only oncologist in a town).
4. **Audio.** Remove it. Voice is an identifier. If the test needs audio, the dataset is PHI and belongs in a BAA-covered environment, not a loose bucket.
5. **Manual sample review.** Read at least a random 200 notes. Any missed identifier means re-run and re-sample. Record the miss rate.
6. **Sign-off.** Privacy Official (Dr. Leah Morgan) signs a short memo: method used, who ran it, what was removed, "no actual knowledge" statement. Keep six years.
7. **Release** into a controlled account, with a no-re-identification clause for anyone with access.

If the team needs exact dates, switch to Expert Determination.

## Common mistakes

- Hashing the MRN and calling it de-identified.
- Keeping full dates "because only year matters to the model".
- Sharing an LDS without a DUA, or for a purpose the rule does not allow.
- A BA de-identifying data for its own product when the BAA is silent.

## Try this

Apply Safe Harbor to this 10-column sample row and write what each column becomes (kept, generalized, or removed), with the reason.

| Column | Value |
| --- | --- |
| `name` | Meera Iyer |
| `mrn` | 00882144 |
| `dob` | 1931-03-14 |
| `service_date` | 2026-04-22 |
| `zip` | 60614 |
| `phone` | 312-555-0142 |
| `icd10` | E11.9 |
| `cpt` | 99214 |
| `provider_npi` | 1234567893 |
| `note_excerpt` | "Pt Meera reports missed doses since daughter moved to Evanston." |

Then answer: is the result still useful for a coding-accuracy test? If not, which method would you choose instead, and what extra paperwork does it need?
