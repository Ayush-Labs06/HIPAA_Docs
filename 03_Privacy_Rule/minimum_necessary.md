# Minimum necessary (§164.502(b), §164.514(d))

When you use, disclose or request PHI, take only what you need for the job. That is the **minimum necessary** standard (45 CFR 164.502(b)(1)). It is a judgment standard, not a fixed list. It also applies to business associates (BAs), including PulseLedger.

## What it requires

| Part | Rule (§164.514(d)) | Plain meaning |
| --- | --- | --- |
| **Uses** (d)(2) | Identify the people or job classes who need access, the categories of PHI they need, and any conditions | Role-based access written down |
| **Routine disclosures** (d)(3)(i) | Standard protocols that limit PHI to what the purpose needs | Same report, same fields, every time |
| **Non-routine disclosures** (d)(3)(ii) | Criteria, and review of each request against them | A human decides |
| **Requests** (d)(4) | Request only what you need; you may rely on a request from a public official, another CE, a professional or a researcher if they say it is the minimum | Do not ask for the whole file |
| **Whole record** (d)(5) | Do not use, disclose or request an entire record unless you can justify it | "All of it" needs a reason |

## When it does NOT apply (§164.502(b)(2))

- Disclosures to or requests by a provider **for treatment**.
- Disclosures **to the individual**.
- Disclosures under a valid **authorization** (limited by what the authorization says).
- Disclosures to **HHS** for enforcement.
- Disclosures **required by law** (§164.512(a)).
- Uses or disclosures required for compliance with the HIPAA Rules.

Everything else, including payment and operations, is covered. This is why a coder does not need the clinical narrative to pick a billing code in every case, and a collections agent does not need diagnosis codes.

HITECH §13405(b) (42 U.S.C. 17935(b)) says to use a **limited data set** where practicable as the minimum. Whether HHS has issued further guidance on that point: check current status.

## Real enforcement

- **Memorial Healthcare System, $5.5 million (OCR resolution agreement, 2017).** OCR found that Memorial gave a former employee of an affiliated physician practice continued access to ePHI (April 2011 to April 2012) and had not implemented procedures to review and modify user access rights. OCR reported PHI of 115,143 individuals was impermissibly accessed. Lesson: "minimum necessary" is empty unless you review who still has access, and audit logs are what make that visible.
- **Pagosa Springs Medical Center, $111,400 (2018).** A former employee kept remote access to a web-based scheduling calendar after leaving; PHI of 557 patients was disclosed to that person. OCR also found a disclosure to the calendar vendor without a BAA. Lesson: offboarding is an access control, and a vendor that holds PHI needs a BAA.

## Role-based access at PulseLedger

Three layers: **who** may touch it, **what fields** they see, **how** they reach it.

| Layer | Control | PulseLedger as-is | Fix |
| --- | --- | --- | --- |
| Who | Client-scoped coder queues; a coder sees only the customers on their team | Queues partly shared across clients | Scope by `client-id`; review quarterly |
| What | Field-level masking: coder sees diagnosis, procedure and encounter facts; not SSN, full address or phone | Full claim record shown | Mask SSN to last 4, hide contact fields from coding roles |
| How | VDI with clipboard, printing and drive redirection off (WorkSpaces) | Chennai allows clipboard and local printing | Disable redirection; no local storage |

Role examples. Eligibility verifier: demographics, member ID, payer, plan. Coder: encounter and diagnosis data, no SSN. Denials analyst: claim, remit and denial codes, no clinical notes. Support agent: account status, masked identifiers, no clinical data. Break-glass engineer: time-boxed access with an approval ticket and full logging.

## An IAM/ABAC sketch

Tag people and data with the same key, then compare. Identity Center passes `client-id` as a session tag, and S3 objects carry a `client-id` tag.

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Sid": "CoderReadsOnlyOwnClientClaims",
    "Effect": "Allow",
    "Action": ["s3:GetObject"],
    "Resource": "arn:aws:s3:::pl-rcm-claims/*",
    "Condition": {
      "StringEquals": {
        "s3:ExistingObjectTag/client-id": "${aws:PrincipalTag/client-id}",
        "s3:ExistingObjectTag/data-class": "ephi",
        "aws:PrincipalTag/role": "coder"
      }
    }
  }]
}
```

Caution: S3 tag-based checks only work if every object is tagged on write; enforce that with a bucket policy or an upstream pipeline step. The full control mapping is in [safeguards_mapped_to_aws.md](../06_Security_Rule/safeguards_mapped_to_aws.md).

## The shared Riverbend login

At one Riverbend EHR, four PulseLedger coders share one customer-issued login. This breaks minimum necessary in practice because you cannot show *who* saw *what*. It also fails the Security Rule's unique user identification standard (§164.312(a)(2)(i)) and weakens audit controls (§164.312(b)).

What to do:

1. Raise it with Dana Alvarez (Riverbend privacy officer): ask for four named accounts under Riverbend's own access process.
2. Until then, add compensating controls: each coder's WorkSpaces session is recorded and tied to their identity, and the shared credential sits in a vault with checkout logging.
3. Record the exception in the risk register ([risk_analysis_with_worked_example.md](../06_Security_Rule/risk_analysis_with_worked_example.md)) with an owner and due date.
4. Do not treat the vault as a permanent fix. The customer's audit log still shows one user.

## Try this

Write a role-to-data access matrix for **five** PulseLedger RCM roles: eligibility verifier, coder, denials analyst, payment poster, and QA contractor. Columns: role, PHI fields visible, fields masked, client scope, access path (VDI or not), review frequency, and a one-line justification. Then answer: which role has the weakest justification for seeing patient date of birth, and what would you do about it?
