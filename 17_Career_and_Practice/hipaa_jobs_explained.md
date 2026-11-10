# HIPAA, healthcare privacy and security jobs, explained (with an India lens)

Most people searching for "HIPAA jobs" find almost nothing under that title. The work exists, but it hides under other names: compliance analyst, GRC analyst, vendor-risk analyst, security assurance. This guide maps the real roles, the credentials that exist, and the India angle. It uses PulseLedger Health as the running example.

> Salary figures are left out on purpose. Pay varies by company, city and year, and no authoritative source was checked for this guide. Look at current postings and salary surveys yourself.

## Why this field hires in India

US healthcare outsources a lot of back-office work: coding, claims, denials, patient messaging, software. Much of that runs from Pune, Chennai, Hyderabad, Noida and other hubs. Each of those operations handles PHI for a US covered entity (CE) as a business associate (BA) or a subcontractor of one. That creates a steady need for people who can keep the BAA chain, access controls and incident handling in order. See [Offshoring PHI to India](../09_Offshore_and_India/offshoring_phi_to_india.md).

## The roles

| Role | What the day looks like | Where you meet it | Most relevant modules |
| --- | --- | --- | --- |
| **Privacy Officer / Privacy Official** | Owns the Privacy Rule program: policies, NPP, individual rights, authorizations, complaints. The rule requires a designated person (§164.530(a)) | CE, or BA with its own program | [Officers](../01_Scope_and_Roles/privacy_and_security_officers.md), [TPO](../03_Privacy_Rule/uses_and_disclosures_tpo.md) |
| **Security Officer / Security Official** | Owns the Security Rule program: risk analysis, safeguards, incident response (§164.308(a)(2)) | CE or BA; often the CISO | [Security Rule](../06_Security_Rule/security_rule_explained.md) |
| **HIPAA compliance analyst** | Runs the risk register, BAAs, access reviews, training records, incident files. This is Priya Nair's job at PulseLedger | BA, health-tech, hospital | [Risk analysis](../06_Security_Rule/risk_analysis_with_worked_example.md), [Program](../14_Compliance_Program_and_Audit/running_a_hipaa_program.md) |
| **GRC analyst, health-tech** | Maps HIPAA to SOC 2, ISO 27001, HITRUST; owns evidence and audits; answers customer questionnaires | SaaS and AI health companies | [Controls and evidence](../14_Compliance_Program_and_Audit/controls_and_evidence.md), [Crosswalk](../14_Compliance_Program_and_Audit/framework_crosswalk.md) |
| **HITRUST assessor or consultant** | Scopes and tests HITRUST assessments, usually at an assessor firm | Assessor firms, consultancies | [Crosswalk](../14_Compliance_Program_and_Audit/framework_crosswalk.md) |
| **BA vendor-risk analyst** | Reviews vendors that touch PHI: BAA status, questionnaires, SOC 2 reports, remediation | CE (hospital, plan) or a large BA | [Vendor management](../08_Business_Associate_Agreements_and_Vendors/vendor_and_cloud_provider_management.md) |
| **Cloud security / privacy engineer** | Builds the controls: encryption, logging, IAM, retention, data mapping | Health-tech engineering | [Safeguards on AWS](../06_Security_Rule/safeguards_mapped_to_aws.md) |
| **RCM/BPO compliance (India)** | Keeps ops floors compliant: client-scoped access, VDI controls, training completion, offshore attestations | Indian RCM and BPO vendors | [Offshore access risk](../09_Offshore_and_India/offshore_access_risk_assessment.md) |

At PulseLedger the split is: Dr. Leah Morgan (Privacy Official, Nashville), Rohan Kulkarni (Security Official, Pune), and Priya in Pune doing the analyst work for both. Small companies often combine roles. Big ones separate them.

## Remote US roles

Some US employers hire remote compliance and security staff, and some hire India-based staff for follow-the-sun coverage. Read each posting for work authorization, hours (US time zones are common) and whether the employer can legally employ you in India. Do not assume "remote" means "open to India". Freelance and contract work (risk analyses, BAA reviews, questionnaire support for small vendors) is another route; clients usually care more about a portfolio than a title. See [Hands-on projects](hands_on_projects.md).

## There is no "HIPAA certification"

No government body issues a HIPAA certification. HHS does not certify people, products, or companies as compliant. Any vendor selling "HIPAA certified" status is selling its own course or seal, which has no legal effect. What exists are professional credentials from independent bodies. They show you studied the field. They do not prove an organization is compliant.

## Credentials that do exist

Names and bodies below were checked against search results in November 2026. Fees, exact experience rules and renewal terms change, so confirm on the issuer's own page before paying.

| Credential | Issuer | What it signals | Notes (check current terms) |
| --- | --- | --- | --- |
| **HCISPP** (HealthCare Information Security and Privacy Practitioner) | ISC2 | Security and privacy in healthcare | ISC2 lists two years of paid experience in the knowledge areas. You can sit the exam first and become an Associate of ISC2, then earn the experience in a set window. Some older sources mention a healthcare-specific year; confirm on ISC2's page |
| **CHPC** (Certified in Healthcare Privacy Compliance) | Compliance Certification Board (CCB), linked to HCCA/SCCE | Privacy compliance | CCB handbook says membership is not required to apply. Eligibility uses work experience plus continuing education; read the current candidate handbook |
| **CHC** (Certified in Healthcare Compliance) | CCB / HCCA | General healthcare compliance (broader than privacy) | Same body and handbook family as CHPC |
| **CHPS** (Certified in Healthcare Privacy and Security) | AHIMA | Privacy and security in health information management | AHIMA's page lists several eligibility routes, including a degree-plus-experience mix; one route is a high school diploma plus six years in healthcare privacy or security |
| **CIPP/US** | IAPP | US private-sector privacy law across all sectors, not only health | A training provider reports no prerequisites; the current body of knowledge took effect 1 September 2025 |
| **HITRUST CCSFP** (Certified Common Security Framework Practitioner) | HITRUST | Ability to work on HITRUST assessments | Relevant if you want assessor or consulting work. Details come from vendor pages; confirm on HITRUST's site |

How to choose:

1. **Targeting India RCM/BPO or health-tech GRC:** a general security credential plus this repo's portfolio often counts for more than a niche health badge. Read the postings.
2. **Targeting US privacy-officer paths:** CHPC, CHPS or CIPP/US fit the privacy side.
3. **Targeting assessor firms:** HITRUST CCSFP and ISO 27001 lead auditor training fit.
4. **No experience yet:** HCISPP's Associate route lets you sit the exam first. Weigh cost against how many postings actually ask for it.

## What employers actually test

Interview panels for these roles tend to ask the same few things. Practice them in [Interview questions](interview_questions.md).

- Can you tell a CE from a BA, and say what each owes? ([Who HIPAA applies to](../01_Scope_and_Roles/who_hipaa_applies_to.md))
- Can you run a risk analysis, not just recite the standard? ([Risk analysis](../06_Security_Rule/risk_analysis_with_worked_example.md))
- Can you decide whether an incident is a breach and name the clocks? ([Is it a breach?](../07_Breaches_and_Incidents/is_it_a_breach.md), [Notification](../07_Breaches_and_Incidents/breach_notification_60_days.md))
- Can you turn rules into cloud controls and evidence? ([Safeguards on AWS](../06_Security_Rule/safeguards_mapped_to_aws.md))
- Can you answer a customer questionnaire without overclaiming? ([OCR and questionnaires](../14_Compliance_Program_and_Audit/ocr_investigations_audits_and_questionnaires.md))

## A realistic path

```
Month 0-2   Read this repo; finish projects 1, 3, 7
Month 2-4   Add the tabletop (5) and the crosswalk (8)
Month 4-6   Publish the portfolio; apply for analyst / junior GRC roles
Ongoing     One credential chosen from postings, not from hype
```

Freelance first clients tend to be small BAs and clinics that need a risk analysis, a BAA cleanup or a questionnaire answered. Say what you did and what you did not do. Never claim a client is "HIPAA compliant" because you finished a project; say what was assessed and when.

## Try this

Find 5 real postings (job boards, company career pages) for privacy, compliance, GRC or vendor-risk roles in health-tech, an RCM/BPO company or a hospital system. For each, copy the requirements into a table with three columns: requirement, which module in this repo covers it, and the gap you still have. Count which credentials appear in at least 3 of the 5, and decide whether that changes your plan.
