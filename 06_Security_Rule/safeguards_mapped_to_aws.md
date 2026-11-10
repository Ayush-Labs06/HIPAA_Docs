# Security Rule safeguards mapped to AWS

No cloud makes you "HIPAA anything". What exists is **HIPAA-eligible services under a BAA**, configured by you. This file turns each Security Rule standard into an AWS control and the evidence you would hand an investigator.

## Shared responsibility and the AWS BAA

- AWS secures the cloud (facilities, hardware, hypervisor). You secure what you put **in** it: IAM, network, encryption, logging, data.
- The BAA is accepted in **AWS Artifact**. Accept it for the organization's management account so member accounts (`pl-prod-rcm`, `pl-prod-connect`, `pl-prod-scribe`, `pl-dev`, `pl-security`, `pl-sandbox`) are covered. Keep the signed copy.
- The BAA covers only **HIPAA-eligible services**. The list is at https://aws.amazon.com/compliance/hipaa-eligible-services-reference/ and changes. Anything outside it must never touch ePHI. PulseLedger has a signed BAA but **nobody ever checked the full list** against what runs in production. Fix: export the services in use (Cost Explorer or Config inventory), compare, and attach the result to the risk register.
- OCR's 2016 cloud guidance: a cloud provider storing ePHI is a BA **even if it cannot read it** (encrypted, no key). This is why the BAA is not optional.

## The mapping table

| Standard | AWS control | Evidence |
| --- | --- | --- |
| Access control §164.312(a)(1) | IAM Identity Center with groups per role; SCPs as guardrails; ABAC using `data-class` and `client` tags | Permission-set export; quarterly access-review ticket |
| Unique user ID (R) | No IAM users or shared roles for people; SSO only; in customer EHRs, named accounts | Identity Center user list; zero-IAM-user Config rule |
| Emergency access (R) | Break-glass role, MFA-gated, alarm on assume-role, reviewed after use | Role policy; CloudTrail event; post-use review |
| Automatic logoff (A) | WorkSpaces session timeout and idle disconnect; app session limits | Directory policy screenshot or export |
| Encryption §164.312(a)(2)(iv), (e)(2)(ii) | KMS CMKs; EBS default encryption; RDS and S3 SSE-KMS; TLS 1.2+ enforced with bucket policy and ALB policy | Config rules `encrypted-volumes`, `s3-bucket-ssl-requests-only`; KMS key policy |
| Audit controls §164.312(b) (R) | Org CloudTrail **with S3 data events** for ePHI buckets; CloudTrail Lake for queries; logs to `pl-security` with Object Lock | Trail config; Lake query; retention setting (6 years) |
| Integrity §164.312(c) | S3 Object Lock, versioning, checksums; CloudTrail log file validation | Bucket configuration; digest validation output |
| Person or entity authentication (R) | MFA for all humans; hardware or passkey MFA for admins | MFA report; IdP policy |
| Transmission security §164.312(e) | TLS to every endpoint; VPN or PrivateLink for clearinghouse links | Scan results; security-group review |
| Contingency plan §164.308(a)(7) | AWS Backup plans with vault lock; cross-region copy to `us-west-2`; restore tests | Backup job reports; restore test record |
| Device and media §164.310(d) | WorkSpaces VDI; clipboard, drive and print redirection off; no local storage | WorkSpaces group policy export |
| Evaluation §164.308(a)(8) | Security Hub standards, Config conformance packs, annual penetration test | Findings export; report |

## Guardrail code (valid Terraform)

Account-level S3 Block Public Access, per account:

```hcl
resource "aws_s3_account_public_access_block" "this" {
  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}

resource "aws_ebs_encryption_by_default" "this" {
  enabled = true
}
```

An SCP that blocks unencrypted EBS and weakening of Block Public Access (attach to the production and dev OUs):

```hcl
resource "aws_organizations_policy" "phi_guardrails" {
  name = "pl-phi-guardrails"
  type = "SERVICE_CONTROL_POLICY"
  content = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Sid      = "DenyUnencryptedVolumes"
        Effect   = "Deny"
        Action   = ["ec2:CreateVolume"]
        Resource = "*"
        Condition = { Bool = { "ec2:Encrypted" = "false" } }
      },
      {
        Sid      = "DenyUnencryptedVolumesOnLaunch"
        Effect   = "Deny"
        Action   = ["ec2:RunInstances"]
        Resource = "arn:aws:ec2:*:*:volume/*"
        Condition = { Bool = { "ec2:Encrypted" = "false" } }
      },
      {
        Sid      = "DenyWeakeningPublicAccessBlock"
        Effect   = "Deny"
        Action   = ["s3:PutAccountPublicAccessBlock", "s3:PutBucketPublicAccessBlock"]
        Resource = "*"
        Condition = {
          StringNotLike = { "aws:PrincipalArn" = "arn:aws:iam::*:role/pl-platform-admin" }
        }
      }
    ]
  })
}
```

Organization trail with S3 data events for ePHI buckets (create in the management or delegated-admin account; the log bucket and key live in `pl-security`):

```hcl
resource "aws_cloudtrail" "org" {
  name                       = "pl-org-trail"
  s3_bucket_name             = aws_s3_bucket.log_archive.id
  is_organization_trail      = true
  is_multi_region_trail      = true
  enable_log_file_validation = true
  kms_key_id                 = aws_kms_key.trail.arn

  advanced_event_selector {
    name = "S3 object-level events for pl- buckets"
    field_selector {
      field  = "eventCategory"
      equals = ["Data"]
    }
    field_selector {
      field  = "resources.type"
      equals = ["AWS::S3::Object"]
    }
    field_selector {
      field       = "resources.ARN"
      starts_with = ["arn:aws:s3:::pl-"]
    }
  }
}
```

Data events cost money. Scope them to ePHI buckets, which is why the `data-class=ephi` tag matters.

## Fixing each as-is gap

| Gap | Fix |
| --- | --- |
| S3 data events off | Trail above; 6-year retention in `pl-security` with Object Lock |
| App logs kept 30 days | Ship to the archive account; set retention by [retention_requirements.md](../10_Retention_Disposal_and_Media/retention_requirements.md) |
| Unencrypted legacy EBS | Snapshot, copy with encryption, restore, delete the old volume; default encryption on |
| No MFA on admin console | Put it behind SSO with MFA; retire it |
| Public bucket risk in `pl-dev` | Account Block Public Access plus the SCP; no production data in dev |
| VDI clipboard and print | Turn redirections off; WorkSpaces group policy |

## What would have prevented INC-2026-024

The bucket `pl-scribe-audio-dev` was public for 31 hours. Account-level Block Public Access would have refused the public policy. The SCP stops anyone turning that block off. And S3 server access logs or CloudTrail data events would have let PulseLedger say who fetched what, instead of being unable to rule access out ([is_it_a_breach.md](../07_Breaches_and_Incidents/is_it_a_breach.md)).

## Try this

Write a single SCP that blocks the three riskiest gaps in the register ([risk_analysis_with_worked_example.md](./risk_analysis_with_worked_example.md)) that an SCP can actually enforce. Hint: unencrypted EBS and public S3 are two; the third could stop anyone turning off CloudTrail (`cloudtrail:StopLogging`, `cloudtrail:DeleteTrail`, `cloudtrail:UpdateTrail`) outside the security role. Run `terraform validate` on it, then say why "no MFA on the app admin console" cannot be fixed with an SCP.
