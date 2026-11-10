# Disposal, media reuse and deletion in the cloud (§164.310(d))

"Delete" means different things on a laptop, in S3, in a backup vault and on paper. HIPAA asks one question for all of them: after you are done, can anyone recover the PHI?

## The standard

Device and media controls, §164.310(d)(1): policies for receipt and removal of hardware and electronic media that contain ePHI, in and out of a facility. Four implementation specifications:

| Spec | R/A | Plain meaning |
| --- | --- | --- |
| Disposal, §164.310(d)(2)(i) | **Required** | Final disposal of ePHI and of the hardware or media that holds it |
| Media re-use, §164.310(d)(2)(ii) | **Required** | Remove ePHI before media is reused |
| Accountability, §164.310(d)(2)(iii) | Addressable | Record who moved which hardware and media |
| Data backup and storage, §164.310(d)(2)(iv) | Addressable | Make a retrievable exact copy before moving equipment |

The Privacy Rule adds a duty to apply reasonable safeguards to PHI, including at disposal (§164.530(c)). That covers paper.

## How to sanitize: NIST SP 800-88

HHS points to NIST SP 800-88 as the benchmark. **Revision 2** (Guidelines for Media Sanitization) was published on **26 September 2025** and replaced Revision 1 (2014), according to the NIST CSRC page; I found a NIST FAQ dated July 2026, so **check current status** for later changes. Its three levels carry over:

| Level | What it does | Example |
| --- | --- | --- |
| **Clear** | Logical overwrite so ordinary tools cannot recover data | Factory reset or overwrite of a reused laptop |
| **Purge** | Makes recovery infeasible even with lab techniques | Cryptographic erase on a self-encrypting drive, block erase on flash |
| **Destroy** | Physically ruins the media | Shredding, disintegration |

Revision 2 focuses more on running a sanitization *programme* (roles, verification, records) than on per-device recipes. That matches what an auditor wants: a procedure, proof it ran, and a record per asset.

## Paper PHI and real disposal cases

OCR treats a dumpster as a breach waiting to happen.

- **New England Dermatology and Laser Center, 2022**: OCR announced a settlement of **US$300,640** and a 2-year corrective action plan. Staff had put empty specimen containers labelled with patient information in a parking-lot dumpster (OCR announcement, 23 August 2022). Lesson: shredding rules cover physical items with labels, not only paper.
- Other disposal resolution agreements exist (for example, OCR's older pharmacy and practice cases involving open containers and dumpsters). I could not verify names and amounts in this session: **check current status** on the HHS resolution agreements page before quoting any other figure.

The lesson for PulseLedger: the Pune and Chennai floors must have locked shred bins and a shred vendor with a certificate of destruction, even if the floor is "paperless".

## Cloud: what "delete" really does

| Layer | What happens on delete | Trap |
| --- | --- | --- |
| **S3, unversioned bucket** | Object removed | Fine, but check replication copies in `us-west-2` |
| **S3 versioned bucket** | A *delete marker* hides the object; older versions remain | PHI still there. Need a lifecycle rule for noncurrent versions |
| **S3 Object Lock** (compliance mode) | Cannot be deleted until retention ends | Good for log archives; **do not** put PHI that must be destroyed in compliance-mode locks |
| **EBS / RDS snapshots** | Separate objects, often forgotten | Inventory and delete snapshots |
| **KMS key deletion** | Key scheduled for deletion after a **7 to 30 day waiting period**; then data encrypted under it is unreadable | This is *crypto-shredding*. It works only if every copy was encrypted with that key |
| **AWS Backup with vault lock** | Locked recovery points cannot be deleted before the min retention | Conflicts with a promise to delete within 30 days. Match vault lock minimums to the shortest deletion promise |

Plan: use a separate customer-scoped KMS key per environment or per tenant tier where cost allows. Then ending a customer, or purging a dev environment, can be proven by key deletion plus listing evidence.

### Terraform lifecycle snippet (audio bucket)

```hcl
resource "aws_s3_bucket_versioning" "audio" {
  bucket = aws_s3_bucket.audio.id
  versioning_configuration { status = "Enabled" }
}

resource "aws_s3_bucket_lifecycle_configuration" "audio" {
  bucket = aws_s3_bucket.audio.id

  rule {
    id     = "expire-audio-30d"
    status = "Enabled"
    filter {}                      # whole bucket

    expiration { days = 30 }

    noncurrent_version_expiration { noncurrent_days = 1 }

    abort_incomplete_multipart_upload { days_after_initiation = 1 }
  }
}
```

Evidence to keep: the plan output, the applied lifecycle configuration (`aws s3api get-bucket-lifecycle-configuration`), and an S3 Inventory report after the expiry date.

## Dev copies: the `pl-dev` audio

INC-2026-024 started with copies of production dictation audio in `pl-dev`. Rule going forward: no production PHI in dev. Use synthetic or de-identified data ([de_identification_and_limited_data_sets.md](../02_PHI_and_Data_Mapping/de_identification_and_limited_data_sets.md)). Prevention is in [safeguards_mapped_to_aws.md](../06_Security_Rule/safeguards_mapped_to_aws.md); the breach side is in [is_it_a_breach.md](../07_Breaches_and_Incidents/is_it_a_breach.md).

## Laptops, VDI and Indian ops centres

- VDI sessions should leave nothing locally. Verify by test, then rely on it.
- Managed laptops: full-disk encryption, MDM remote wipe, an asset register with owner and serial number (accountability spec).
- At disposal: cryptographic erase or overwrite per 800-88, then record serial number, method, operator, date and verifier.
- E-waste vendors in India: use a vendor that issues a **certificate of destruction** per serial number and agrees to a BAA or equivalent confidentiality terms for any media that may hold PHI. Physically destroy drives that failed erase.
- Contractor devices: no PHI on personal laptops, so nothing to dispose of. If that rule was broken, require written attestation and wipe verification.

## Certificate of destruction: minimum fields

Asset ID or serial, media type, PHI category, method (Clear/Purge/Destroy), tool and version, date, operator, verifier, witness, and the destruction vendor's signed statement.

## Try this

Write the **runbook to purge production audio from `pl-dev`** with evidence:

1. Freeze: block new writes, confirm the bucket is private and logging is on.
2. Inventory: S3 Inventory plus Macie job to list what exists.
3. Delete current objects, delete all versions and delete markers, check replication.
4. Snapshots and EBS volumes that held copies: list and delete.
5. Schedule deletion of the KMS key used on the bucket (state the waiting period you choose).
6. Prove it: post-purge inventory showing zero objects, CloudTrail events for the deletes, ticket sign-off by Rohan Kulkarni.
7. Record the actions for the INC-2026-024 file.
