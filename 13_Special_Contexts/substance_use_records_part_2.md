# Substance use disorder records: 42 CFR Part 2

Part 2 is a second privacy law that sits next to HIPAA. It protects records of people treated for substance use disorder (SUD) at certain federally assisted programs. It is older than HIPAA, stricter in some places, and it is the reason one LedgerRCM customer needs special handling.

## Who Part 2 covers

Part 2 applies to a **Part 2 program** (42 CFR 2.11, 2.12): a federally assisted program that holds itself out as providing SUD diagnosis, treatment or referral. "Federally assisted" is broad: a DEA registration, tax-exempt status, Medicare or Medicaid participation, or federal funding all count.

| Entity | Part 2 status | HIPAA status |
| --- | --- | --- |
| Lakeshore Behavioral Health SUD clinic | Part 2 program | Covered entity (CE) |
| Lakeshore general behavioral clinics (no SUD programme) | Not Part 2 | CE |
| Lakeshore's payer, receiving claims | Lawful holder (via consent) | CE |
| PulseLedger running claims for the SUD clinic | Qualified service organization (QSO) | Business associate (BA) |
| A general hospital with no SUD unit | Not a Part 2 program | CE |

Part 2 follows the **record**, not just the entity. A recipient who gets Part 2 records under a patient consent is bound by the redisclosure limits in the rule.

## The 2024 final rule: Part 2 meets HIPAA

HHS and SAMHSA published the final rule on 16 February 2024 (89 FR 12472), mandated by the CARES Act (section 3221). It moved Part 2 closer to HIPAA. As of November 2026:

- **Effective 16 April 2024. Compliance date 16 February 2026**, confirmed by the HHS fact sheet. That date has passed.
- OCR announced a Part 2 civil enforcement programme around 13 February 2026 (reported by law firms; check the HHS Part 2 page for the primary notice). HIPAA's Enforcement Rule (45 CFR Part 160, Subparts C–E) now applies to Part 2 violations, so HIPAA civil money penalties are possible.
- **One consent for all future TPO.** A patient can sign a single consent covering treatment, payment and health care operations (§2.31). Recipients that are CEs or BAs may then use and redisclose the records as HIPAA allows, with the exception below.
- **Breach notification** under the HIPAA Breach Notification Rule now applies to breaches of unsecured Part 2 records (§2.16), even for a Part 2 program that is not otherwise a CE.
- **Notice of Privacy Practices.** Part 2 programs must update their Part 2 notice (§2.22). The Part 2 NPP changes were not vacated when the reproductive-health rule fell in *Purl v. HHS* (N.D. Tex., 18 June 2025). See [../04_Notice_of_Privacy_Practices/notice_of_privacy_practices.md](../04_Notice_of_Privacy_Practices/notice_of_privacy_practices.md).
- Patients get an accounting of disclosures and a right to opt out of fundraising, closer to HIPAA.

What did **not** change: records still cannot be used against the patient in civil, criminal, administrative or legislative proceedings without the patient's written consent or a court order meeting Subpart E (§2.12(d), §2.64–2.66). A subpoena alone is never enough. Check the text of §2.3 for the good-faith safe harbor given to investigative agencies.

## QSOA vs BAA

| | BAA (HIPAA) | QSOA (Part 2) |
| --- | --- | --- |
| Authority | §164.504(e) | §2.11, §2.12(c)(4) |
| Who signs | CE and BA | Part 2 program and QSO |
| Key promises | Safeguards, breach notice, subcontractor flow-down | Acknowledge Part 2 binds you; resist unauthorized disclosure; follow Part 2 on redisclosure |
| Typical practice | Separate document | Combine: one BAA with a Part 2 addendum |

A BA that handles Part 2 records for a program should have both promises in writing. Add to the BAA: "Business Associate acknowledges it is fully bound by 42 CFR Part 2 when receiving, storing, processing or otherwise dealing with Part 2 records, and will resist in judicial proceedings any effort to obtain access except as Part 2 permits." Review clause by clause in [../08_Business_Associate_Agreements_and_Vendors/baa_clause_by_clause_review.md](../08_Business_Associate_Agreements_and_Vendors/baa_clause_by_clause_review.md).

## Segmentation and tagging (DS4P ideas)

HL7 **Data Segmentation for Privacy (DS4P)** is a standard way to label data with its sensitivity and the redisclosure rules that travel with it. You do not need to implement full DS4P to benefit. You need three things: **tag at ingestion, enforce at access, carry the label out**.

```
Lakeshore EHR ──837/835──► ingest ──► classifier ──► LedgerRCM store
                              │           │
          facility ID ∈ part2_programs?   │  yes → tag part2=true, program_id=LSB-014
                                          │  no  → tag part2=false
                                          ▼
                           KMS key: pl-part2-key (separate CMK)
                           S3 prefix: s3://pl-rcm-claims/part2/
```

Tags and rules for PulseLedger:

| Tag | Values | Rule |
| --- | --- | --- |
| `data-class` | `ephi` | Baseline for every claim |
| `part2` | `true` / `false` | Set from the customer's Part 2 program list, not guessed from CPT codes |
| `part2-program-id` | Lakeshore's internal ID | Used for consent lookup |
| `consent-tpo` | `on-file` / `none` | From Lakeshore's consent record |

```hcl
# Only principals tagged for Part 2 work may read part2 objects
data "aws_iam_policy_document" "part2_read" {
  statement {
    effect    = "Allow"
    actions   = ["s3:GetObject"]
    resources = ["arn:aws:s3:::pl-rcm-claims/part2/*"]
    condition {
      test     = "StringEquals"
      variable = "aws:PrincipalTag/part2-trained"
      values   = ["true"]
    }
  }
}
```

Controls that go with the tags: a separate KMS key so access can be revoked as a unit; a named, trained coder pool; a log alert on every read; a payer-disclosure check against the consent flag before any 837 leaves for the clearinghouse; and a rule that Part 2 data is **never** copied to `pl-dev`.

## PulseLedger as-is and the fix

As-is: Lakeshore's SUD records sit mixed in general LedgerRCM data with no tag. Nobody can tell a coder, a regulator or an auditor which rows are Part 2. Fix order: (1) get Lakeshore's program list; (2) back-fill tags with a one-off job and a reviewed sample; (3) split storage and keys; (4) add the QSOA clauses; (5) train the Part 2 coder pool.

**INC-2026-031 Part 2 angle.** Of the 61,400 people, **3,900** are Lakeshore SUD patients. Consequences: Lakeshore is both CE and Part 2 program, so it carries the breach notification duty for those patients, and PulseLedger must tell Lakeshore fast (their paper: 5 days). The notice letters must not reveal SUD treatment on an envelope or in a subject line. The mixed storage meant PulseLedger could not instantly list who was in the 3,900; that delay is itself a finding. Timelines are in [../07_Breaches_and_Incidents/breach_notification_60_days.md](../07_Breaches_and_Incidents/breach_notification_60_days.md) and the tabletop in [../07_Breaches_and_Incidents/ransomware_tabletop_exercise.md](../07_Breaches_and_Incidents/ransomware_tabletop_exercise.md). State law may add a stricter SUD rule: check each state's current status.

## Real enforcement

OCR's Part 2 enforcement only began in 2026. As of November 2026 I found no published Part 2 resolution agreement: check the OCR enforcement highlights page for current status. Until then, assume HIPAA-style investigations, built on HIPAA-style risk analysis findings.

## Try this

Design the data tags and access rules for Part 2 records in LedgerRCM. Deliver a one-page table: the tags, who sets each, who may read each, which log proves it, and what happens when `consent-tpo=none` and a payer asks for the claim. Then write the three-line QSOA clause you would add to the Lakeshore BAA.
