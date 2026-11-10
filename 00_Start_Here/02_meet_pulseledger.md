# Meet PulseLedger Health (the company we use for every example)

Abstract rules are hard to remember. So every module applies them to one fictional company, built to hit the problems you will face at a US health-tech business associate with a large Indian workforce.

> PulseLedger Health is invented. Any resemblance to a real company is accidental.

## The company

| | |
| --- | --- |
| Founded | 2018, Pune |
| Entities | **PulseLedger Health Inc** (Delaware; HQ Nashville, Tennessee: sales, compliance, legal, US customer success) · **PulseLedger Technologies Pvt Ltd** (Pune: engineering, RCM operations, coding). Ops centres in Pune and Chennai |
| People | 640 employees: 560 in India (380 RCM operations and certified coders, 150 engineering, 30 other), 80 in the US. Plus 45 freelance coding-QA contractors in India via a staffing firm |
| Revenue | ~US$46 million ARR |
| PHI held | ~9.2 million patients |
| Hosting | AWS under an AWS BAA. Production `us-east-1`, DR `us-west-2`. Indian staff reach production through an AWS-hosted VDI (Amazon WorkSpaces) in `us-east-1` |
| AWS accounts | `pl-prod-rcm`, `pl-prod-connect`, `pl-prod-scribe`, `pl-dev`, `pl-security` (log archive), `pl-sandbox` |

## Products

1. **LedgerRCM**: revenue cycle management. Eligibility checks, coding by offshore certified coders, claim scrubbing, submission through a US clearinghouse, denials and payment posting. Handles X12 837, 835 and 270/271.
2. **PulseConnect**: appointment reminders (SMS, email) and a bill-pay portal at `pay.pulseconnect.example`. Marketing site: `pulseledger.example`.
3. **ScribeAssist** (launched June 2026): clinician dictation audio is transcribed, then a third-party LLM drafts a SOAP note and suggests ICD-10/CPT codes. The LLM vendor is US-based, with a BAA and zero data retention.
4. **PulseWell** (launched 2025): a consumer symptom and medication tracker. Consumers sign up themselves; no provider is involved. So it is **not** HIPAA. FTC and state consumer-health laws apply instead.

## How PHI flows

```
Provider EHR ──▶ LedgerRCM (pl-prod-rcm) ──837──▶ US clearinghouse ──▶ Payer
      ▲               │  ▲                                              │
      └───────────────┘  └──────────────835 / 271───────────────────────┘
                       │
                       └──▶ Coders in Pune/Chennai via WorkSpaces VDI

Clinician dictation ──▶ Speech-to-text vendor ──▶ LLM vendor ──▶ SOAP note + codes
   (pl-prod-scribe)       (no BAA yet)           (BAA, zero retention)

Patient ◀── SMS / email gateways ◀── PulseConnect (pl-prod-connect) ◀── Provider schedule
Patient ──▶ pay.pulseconnect.example (bill-pay)

Consumer ──▶ PulseWell app (own servers, not HIPAA)

App logs (names + MRNs) ──▶ LogPeak (no BAA)     Dev copies ──▶ pl-dev
```

## Roles

- **Business associate (BA)** to every provider customer for LedgerRCM, PulseConnect and ScribeAssist.
- **Not** a covered entity. Its US staff's group health plan is fully insured through a carrier.
- PulseLedger Technologies Pvt Ltd is a **subcontractor BA** of PulseLedger Health Inc. An intercompany BAA is missing.

## Customers (~240 covered entities)

| Customer | Notes |
| --- | --- |
| **Riverbend Health System** (Ohio) | 6 hospitals; largest customer; privacy officer Dana Alvarez |
| **Lakeshore Behavioral Health** (Illinois) | 9 clinics; includes a federally assisted SUD programme, so **42 CFR Part 2** records flow into LedgerRCM |
| **Summit Valley Health Plan** (Arizona) | Medicare Advantage **prospect**; 280-question questionnaire plus a CMS offshore-subcontractor attestation request |
| ~180 physician practices · ~40 behavioural health clinics · ~15 dental groups | Mostly LedgerRCM + PulseConnect; some clinics run Part 2 programmes |

## Vendors (all fictional or generic)

| Vendor | Use | BAA (as-is) |
| --- | --- | --- |
| AWS | Hosting | Signed; no one has checked the HIPAA-eligible list |
| US clearinghouse | Claim routing | Signed |
| SMS gateway, transactional email vendor | PulseConnect | Signed |
| LLM provider | ScribeAssist | Signed + zero retention |
| Speech-to-text vendor | Transcription | **None** (pilot contract) |
| **LogPeak** (log-analytics SaaS) | App logs with names + MRNs | **None** |
| Company chat tool | Internal chat | Plan has **no BAA** |
| Google Workspace | Email/docs | Signed; core services only |
| Staffing firm | Coding-QA contractors | **None**; contractors use personal laptops in one workflow |

## The people you will "meet"

| Person | Role | Cares about |
| --- | --- | --- |
| Dr. Leah Morgan | Chief Compliance & Privacy Officer; **HIPAA Privacy Official** (Nashville) | Defensible decisions, customer trust |
| Rohan Kulkarni | CISO; **HIPAA Security Official** (Pune) | Risk analysis, controls, SOC 2 + HITRUST |
| Priya Nair | HIPAA Compliance Analyst (Pune), **this is you** | Risk register, BAAs, access reviews, incidents |
| Marcus Webb | General Counsel (Nashville) | BAA negotiations, breach liability |
| Sneha Joshi | VP Engineering (Pune) | Shipping ScribeAssist v2 |
| Farhan Sheikh | Head of RCM Operations | Coder productivity, client SLAs |
| Dana Alvarez | Privacy Officer at Riverbend (customer) | Her obligations as a CE |
| Jason Pike | Enterprise AE | Closing Summit Valley |

## The starting position (as-is, January 2026)

- Last "risk analysis" was a 2021 vendor-questionnaire checklist. ScribeAssist, PulseWell and Chennai were never in scope.
- BAA template from 2014: no subcontractor flow-down, vague breach timing. ~30 customers use their own paper with clocks of 24 hours, 5 days, 10 days and "without unreasonable delay".
- No intercompany BAA; none with LogPeak, the speech-to-text vendor or the staffing firm.
- Chennai VDI allows clipboard and local printing. At one Riverbend EHR, 4 coders share one login.
- CloudTrail on, **S3 data events off**. App audit logs kept **30 days**.
- Legacy LedgerRCM admin console has **no MFA**.
- A legacy EC2 MySQL host has an **unencrypted EBS volume** with 2019-2021 claim archives.
- The bill-pay portal, including login and authenticated pages, loads a **Meta Pixel and Google Analytics**.
- Training is an annual video; no completion records for Chennai contractors.
- Policies written in 2019, never reviewed. Nobody knows the documentation retention period.
- Patient access requests forwarded by customers sit in a shared inbox.
- Lakeshore's Part 2 records are mixed into general LedgerRCM data, untagged.
- `pl-dev` holds **copies of production dictation audio**.

You will handle **five incidents** this year. Their details live in [07_Breaches_and_Incidents](../07_Breaches_and_Incidents/is_it_a_breach.md).

## Why PulseLedger is a good teaching case

1. **BA, not CE, and a consumer app next door.** Who is covered at all? → [who_hipaa_applies_to.md](../01_Scope_and_Roles/who_hipaa_applies_to.md)
2. **A BA chain with holes.** → [business_associates_explained.md](../01_Scope_and_Roles/business_associates_explained.md), [baa_clause_by_clause_review.md](../08_Business_Associate_Agreements_and_Vendors/baa_clause_by_clause_review.md)
3. **Shadow PHI in logs, chat, dev.** → [ephi_data_mapping_in_the_cloud.md](../02_PHI_and_Data_Mapping/ephi_data_mapping_in_the_cloud.md)
4. **Offshore access.** → [offshoring_phi_to_india.md](../09_Offshore_and_India/offshoring_phi_to_india.md)
5. **A stale risk analysis.** → [risk_analysis_with_worked_example.md](../06_Security_Rule/risk_analysis_with_worked_example.md)
6. **Pixels on a bill-pay portal.** → [tracking_technologies.md](../11_Tracking_Tech_and_Health_Apps/tracking_technologies.md)
7. **An AI product.** → [ai_with_phi.md](../12_AI_and_Health_Data/ai_with_phi.md)
8. **Part 2 records mixed in.** → [substance_use_records_part_2.md](../13_Special_Contexts/substance_use_records_part_2.md)
9. **Patient requests in a shared inbox.** → [right_of_access.md](../05_Individual_Rights/right_of_access.md)

## Questions you will be able to answer by the end

- Is PulseLedger a covered entity, a business associate, or both? Is PulseWell covered at all?
- Can Pune and Chennai coders access PHI, and under what conditions? What does Summit Valley's attestation require?
- Is the Chennai ransomware incident a breach? Who notifies whom, by when? Does Part 2 change anything?
- Can ScribeAssist use customer PHI to fine-tune or evaluate models?
- A patient emails PulseLedger asking for "all my records". What happens?
- What goes in the reply to Summit Valley's questionnaire?

## Try this

Using the diagram, list every place PHI **enters**, **rests** and **leaves** PulseLedger. Make a three-column table. Include the shadow places (LogPeak, chat, `pl-dev`, contractor laptops). Mark each with its BAA status. You will turn this into a proper inventory in [ephi_data_mapping_in_the_cloud.md](../02_PHI_and_Data_Mapping/ephi_data_mapping_in_the_cloud.md).
