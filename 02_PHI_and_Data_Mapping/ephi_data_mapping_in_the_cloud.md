# Mapping ePHI in a cloud company (how it's really done)

You cannot protect, log, retain or breach-assess data you do not know you have. The first job in any HIPAA program is a map: where ePHI enters, rests, moves and leaves. At a cloud company the map goes stale in weeks, so it must be built from tooling, not just a spreadsheet.

## Why a map, and what the law says

- The Security Rule's risk analysis (45 CFR §164.308(a)(1)(ii)(A)) must cover **all** ePHI the organization creates, receives, maintains or transmits (§164.306(a)). OCR's own risk analysis guidance says to identify where ePHI lives. You cannot scope the analysis without the map. See [risk_analysis_with_worked_example.md](../06_Security_Rule/risk_analysis_with_worked_example.md).
- The 2025 Security Rule NPRM (90 FR 898, 6 Jan 2025) proposes making a **technology asset inventory and network map** an explicit written requirement. Status as of November 2026: still a proposal; trade press reports the HHS regulatory agenda moved final action to July 2027. Check current status on reginfo.gov before relying on it. The current rule is in force either way, and a map is how you meet it today.
- A map also drives breach scoping (how many people, which states), retention, offboarding and BAA coverage.

## Three views, one source of truth

| View | Question | Artifact |
| --- | --- | --- |
| **Asset inventory** | What systems and stores hold ePHI? | Table of systems (below), fed by AWS Config |
| **Data-flow diagram** | How does ePHI move between them and outside? | One diagram per product |
| **People-and-access map** | Who can reach it? | IAM Identity Center assignments, customer EHR logins |

## Discovery methods (use several; each misses something)

| Method | Finds | Misses |
| --- | --- | --- |
| **AWS Config** (aggregator across all accounts) | Every S3 bucket, EBS volume, RDS instance, with config history | What is *inside* them |
| **Resource tags** (`data-class=ephi`) | Owner's declared classification | Untagged resources |
| **Amazon Macie** | Sensitive data in S3: names, SSNs, medical identifiers | Non-S3 stores, audio, odd formats |
| **Interviews** (engineering, RCM ops, support, sales) | Spreadsheets, chat paste, laptops, vendors | Anything people forget |
| **Log and egress review** (VPC flow logs, firewall, DNS, SaaS audit logs) | Undeclared destinations such as an unknown SaaS | Encrypted payload contents |
| **Vendor and spend review** (AP invoices, SSO app list) | Shadow SaaS | Free tiers |
| **Code and config search** | Hard-coded endpoints, logging statements printing PHI | Runtime-only paths |

Rule of thumb: AWS Config finds the *containers*, Macie finds the *contents*, interviews find the *surprises*.

## Data-flow sketch: LedgerRCM claim

```
Provider EHR ──SFTP/TLS──▶ pl-prod-rcm S3 (SSE-KMS) ──▶ coder queue DB
                                                       │
         Pune/Chennai coder ──WorkSpaces VDI──────────┘
                                                       │
                                  claim 837 ──TLS──▶ US clearinghouse (BAA)
                                                       │
         App logs (names + MRNs) ────────────────────▶ LogPeak (NO BAA)
         Nightly copy ──▶ pl-dev  (unmasked)  <-- shadow copy
```

Draw one of these per product, and mark every arrow that crosses an account, a vendor or a border. Those crossings are where the BAA and the offshore questions live.

## Sample ePHI inventory for PulseLedger

Figures are illustrative planning estimates for the teaching case, not frozen company facts.

| System | AWS account | Data | Volume (est.) | Owner | BAA | Encryption | Logging |
| --- | --- | --- | --- | --- | --- | --- | --- |
| LedgerRCM claims store | `pl-prod-rcm` | 837/835/270/271, demographics, CPT/ICD | ~9.2M patients | Farhan Sheikh | AWS BAA | KMS | CloudTrail on, **S3 data events off** |
| Legacy MySQL claim archive (EC2) | `pl-prod-rcm` | 2019-2021 claims | ~2 TB | Sneha Joshi | AWS BAA | **EBS unencrypted** | OS logs only |
| PulseConnect message queue and templates | `pl-prod-connect` | Name, phone, appointment, provider | ~1M msgs/month | Sneha Joshi | AWS, SMS and email vendors | KMS | App logs 30 days |
| Bill-pay portal | `pl-prod-connect` | Name, balance, payment status, IP | Session-level | Sneha Joshi | AWS | TLS + KMS | Web logs; **pixel to Meta/Google** |
| Application logs | LogPeak (SaaS) | Names, MRNs in stack traces | ~50 GB/day | Rohan Kulkarni | **None** | Vendor-side | Vendor audit unknown |
| Dev audio copy | `pl-dev` | Dictation audio, production origin | Several hundred GB | Sneha Joshi | AWS | Default SSE | S3 access logs **off** |
| Company chat tool | SaaS | Pasted claim extracts (INC-2026-027) | Unknown | IT | **None on plan** | Vendor-side | Vendor-side |
| Chennai file server | On-prem | Claims export files | Tens of GB | Farhan Sheikh | n/a | Not encrypted at rest | Firewall only |
| Contractor laptops (QA workflow) | Personal devices | Screenshots, downloads | Unknown | Farhan Sheikh | **None (staffing firm)** | Unknown | None |
| Google Workspace | SaaS | Email attachments, docs | Unknown | IT | BAA, core services | Vendor-side | Admin audit log |

The ScribeAssist row is left for you in **Try this**.

## Shadow PHI: where the map usually fails

1. **Logs.** Exceptions and debug lines print request bodies. LogPeak is a BA the moment it stores names and MRNs, with or without a contract. Fix the code first (redact, tokenize), then the vendor.
2. **Chat and email.** A support engineer pasted a claims extract into a chat tool with no BAA (INC-2026-027, 23 patients).
3. **Dev and test copies.** Production audio sitting in `pl-dev` led to INC-2026-024. Dev does not stop being PHI because it is dev.
4. **Laptops, downloads, screenshots.** VDI clipboard and print rules at Chennai matter here.
5. **Backups, snapshots, DR region** (`us-west-2`). Easy to forget and often outside the tag policy.
6. **Vendor-held copies.** Clearinghouse, SMS gateway, LLM provider and speech-to-text vendor all hold PHI. The speech-to-text vendor has no BAA yet.

## Terraform: enforce the `data-class` tag

A tag policy in AWS Organizations standardises tag keys and values. It tells you *how* things are labelled; Config rules or SCPs enforce that the tag exists.

```hcl
resource "aws_organizations_policy" "data_class" {
  name        = "data-class-tag-policy"
  description = "Standard data-class tag for ePHI discovery"
  type        = "TAG_POLICY"

  content = jsonencode({
    tags = {
      "data-class" = {
        tag_key = {
          "@@assign" = "data-class"
        }
        tag_value = {
          "@@assign" = ["ephi", "deidentified", "internal", "public"]
        }
        enforced_for = {
          "@@assign" = ["s3:bucket"]
        }
      }
    }
  })
}

resource "aws_organizations_policy_attachment" "data_class_root" {
  policy_id = aws_organizations_policy.data_class.id
  target_id = var.organization_root_id
}

variable "organization_root_id" {
  type        = string
  description = "ID of the AWS Organizations root or OU to attach the tag policy to"
}
```

Add a Macie job so the tags are checked against contents:

```hcl
resource "aws_macie2_account" "this" {}

resource "aws_macie2_classification_job" "prod_rcm_weekly" {
  name       = "pl-prod-rcm-weekly-scan"
  job_type   = "SCHEDULED"
  depends_on = [aws_macie2_account.this]

  schedule_frequency {
    weekly_schedule = "MONDAY"
  }

  s3_job_definition {
    bucket_definitions {
      account_id = var.prod_rcm_account_id
      buckets    = var.prod_rcm_buckets
    }
  }
}

variable "prod_rcm_account_id" {
  type = string
}

variable "prod_rcm_buckets" {
  type = list(string)
}
```

Check which resource types your tag policy can enforce in the AWS docs before widening `enforced_for`. Confirm Macie is a HIPAA-eligible service on AWS's current list, as with every service you store ePHI in (see [safeguards_mapped_to_aws.md](../06_Security_Rule/safeguards_mapped_to_aws.md)).

## Keeping it alive

- Re-run discovery quarterly and on every new product or vendor. ScribeAssist (June 2026) and the Chennai centre were absent from the 2021 analysis, which is exactly how maps rot.
- Make "tag + owner + BAA + data-flow entry" part of the launch checklist for any new bucket, database or SaaS.
- Store each map version with a date. Documentation must be kept six years (§164.316(b)(2)).

## Try this

Build the inventory row for **ScribeAssist** using the same eight columns. Cover three separate stores: the audio landing bucket in `pl-prod-scribe`, the transcript and SOAP-note store, and the prompt/response path to the LLM vendor. For each, fill system, account, data, owner (Sneha Joshi), BAA status (speech-to-text vendor: none yet), encryption and logging. Then list two shadow copies (hint: `pl-dev`, and prompt logs).
