# HIPAA regulation quick map (45 CFR Parts 160 and 164)

Use this as an index: find the section, read the topic, jump to the module. The law itself is on eCFR: [Part 160](https://www.ecfr.gov/current/title-45/part-160) and [Part 164](https://www.ecfr.gov/current/title-45/part-164). eCFR shows the text in force today, so check it for amendments (for example, the 2025 Security Rule proposal is **not** final as of November 2026; see [security_rule_explained.md](../06_Security_Rule/security_rule_explained.md)).

## Part 160: general and enforcement

| Section | Topic | Module |
| --- | --- | --- |
| §160.101-102 | Purpose and applicability | [what HIPAA is](../00_Start_Here/01_what_hipaa_actually_is.md) |
| §160.103 | Definitions: CE, BA, workforce, PHI, subcontractor | [who it applies to](../01_Scope_and_Roles/who_hipaa_applies_to.md), [BAs](../01_Scope_and_Roles/business_associates_explained.md) |
| §160.201-205 | Preemption; §160.203 "more stringent" state law survives | [state laws](../15_Related_Laws/state_health_privacy_laws.md) |
| §160.306-312 | Complaints, compliance reviews, cooperation, informal resolution and CAPs | [investigations](../14_Compliance_Program_and_Audit/ocr_investigations_audits_and_questionnaires.md) |
| §160.402-416 | CMP basis, amounts and the four tiers (§160.404), factors (§160.408), defenses, limitations, proposed determination | [penalties](../16_Enforcement_and_Case_Law/ocr_penalties_and_how_enforcement_works.md) |
| §160.500 and after | Hearings before an ALJ (Subpart E) | [penalties](../16_Enforcement_and_Case_Law/ocr_penalties_and_how_enforcement_works.md) |

## Part 164 Subpart A: general

| Section | Topic | Module |
| --- | --- | --- |
| §164.102-106 | Statutory basis, definitions, applicability | [what HIPAA is](../00_Start_Here/01_what_hipaa_actually_is.md) |
| §164.103, §164.105 | Hybrid entities, affiliated covered entities (ACE), organized health care arrangements (OHCA) | [who it applies to](../01_Scope_and_Roles/who_hipaa_applies_to.md) |

## Part 164 Subpart C: Security Rule

| Section | Topic | Module |
| --- | --- | --- |
| §164.306 | General requirements; flexibility factors | [Security Rule](../06_Security_Rule/security_rule_explained.md) |
| §164.308(a)(1)(ii)(A)-(D) | Risk analysis; risk management; sanction policy; activity review | [risk analysis](../06_Security_Rule/risk_analysis_with_worked_example.md) |
| §164.308(a)(2) | Assigned security responsibility | [officers](../01_Scope_and_Roles/privacy_and_security_officers.md) |
| §164.308(a)(3)-(4) | Workforce security; information access management | [Security on AWS](../06_Security_Rule/safeguards_mapped_to_aws.md) |
| §164.308(a)(5) | Security awareness and training | [running a program](../14_Compliance_Program_and_Audit/running_a_hipaa_program.md) |
| §164.308(a)(6) | Security incident procedures | [is it a breach](../07_Breaches_and_Incidents/is_it_a_breach.md) |
| §164.308(a)(7) | Contingency plan (backup, DR, emergency mode) | [Security on AWS](../06_Security_Rule/safeguards_mapped_to_aws.md) |
| §164.308(a)(8) | Evaluation (periodic technical and non-technical) | [controls and evidence](../14_Compliance_Program_and_Audit/controls_and_evidence.md) |
| §164.308(b) | BA contracts (Security Rule side) | [BAA review](../08_Business_Associate_Agreements_and_Vendors/baa_clause_by_clause_review.md) |
| §164.310 | Physical safeguards; (d) device and media controls | [disposal](../10_Retention_Disposal_and_Media/disposal_and_media_reuse.md) |
| §164.312(a)-(e) | Access control, audit controls (b), integrity, authentication, transmission security | [Security on AWS](../06_Security_Rule/safeguards_mapped_to_aws.md) |
| §164.314 | Organizational requirements (BA contracts) | [BAA review](../08_Business_Associate_Agreements_and_Vendors/baa_clause_by_clause_review.md) |
| §164.316 | Policies and documentation; (b)(2) six-year retention | [retention](../10_Retention_Disposal_and_Media/retention_requirements.md) |

## Part 164 Subpart D: Breach Notification

| Section | Topic | Module |
| --- | --- | --- |
| §164.400-402 | Applicability; definition of breach; unsecured PHI | [is it a breach](../07_Breaches_and_Incidents/is_it_a_breach.md) |
| §164.404-408 | Notice to individuals, media, and the Secretary | [notification](../07_Breaches_and_Incidents/breach_notification_60_days.md) |
| §164.410 | Notice by a business associate to the CE | [notification](../07_Breaches_and_Incidents/breach_notification_60_days.md) |
| §164.412-414 | Law-enforcement delay; burden of proof | [notification](../07_Breaches_and_Incidents/breach_notification_60_days.md) |

## Part 164 Subpart E: Privacy Rule

| Section | Topic | Module |
| --- | --- | --- |
| §164.500-501 | Applicability; definitions (marketing, health care operations) | [authorizations](../03_Privacy_Rule/authorizations_and_special_permissions.md) |
| §164.502(a) | Default rule: permitted or required uses only | [TPO](../03_Privacy_Rule/uses_and_disclosures_tpo.md) |
| §164.502(b), §164.514(d) | Minimum necessary | [minimum necessary](../03_Privacy_Rule/minimum_necessary.md) |
| §164.502(e), §164.504(e) | Business associate contracts | [BAs](../01_Scope_and_Roles/business_associates_explained.md), [BAA review](../08_Business_Associate_Agreements_and_Vendors/baa_clause_by_clause_review.md) |
| §164.502(g) | Personal representatives | [minors](../13_Special_Contexts/minors_representatives_and_sensitive_care.md) |
| §164.504(f) | Group health plan and plan sponsor | [employers](../13_Special_Contexts/employers_and_group_health_plans.md) |
| §164.506 | Treatment, payment, health care operations | [TPO](../03_Privacy_Rule/uses_and_disclosures_tpo.md) |
| §164.508 | Authorizations; psychotherapy notes; marketing; sale of PHI | [authorizations](../03_Privacy_Rule/authorizations_and_special_permissions.md) |
| §164.510 | Opportunity to agree or object (family) | [TPO](../03_Privacy_Rule/uses_and_disclosures_tpo.md) |
| §164.512 | Twelve public-interest categories; (i) research | [public interest](../03_Privacy_Rule/public_interest_disclosures.md) |
| §164.514(a)-(c) | De-identification: Safe Harbor, Expert Determination, re-identification codes | [de-identification](../02_PHI_and_Data_Mapping/de_identification_and_limited_data_sets.md) |
| §164.514(e) | Limited data set and data use agreement | [de-identification](../02_PHI_and_Data_Mapping/de_identification_and_limited_data_sets.md) |
| §164.520 | Notice of Privacy Practices | [NPP](../04_Notice_of_Privacy_Practices/notice_of_privacy_practices.md) |
| §164.522 | Restrictions; confidential communications | [restrictions](../05_Individual_Rights/restrictions_confidential_comms_and_complaints.md) |
| §164.524 | Access | [right of access](../05_Individual_Rights/right_of_access.md) |
| §164.526 | Amendment | [amendment and accounting](../05_Individual_Rights/amendment_and_accounting_of_disclosures.md) |
| §164.528 | Accounting of disclosures | [amendment and accounting](../05_Individual_Rights/amendment_and_accounting_of_disclosures.md) |
| §164.530(a) | Privacy Official | [officers](../01_Scope_and_Roles/privacy_and_security_officers.md) |
| §164.530(b) | Training | [running a program](../14_Compliance_Program_and_Audit/running_a_hipaa_program.md) |
| §164.530(d), (g)-(h) | Complaints; non-retaliation; no waiver of rights | [restrictions and complaints](../05_Individual_Rights/restrictions_confidential_comms_and_complaints.md) |
| §164.530(e)-(f) | Sanctions; mitigation | [risk analysis](../06_Security_Rule/risk_analysis_with_worked_example.md) |
| §164.530(i)-(j) | Policies; documentation, six-year retention | [retention](../10_Retention_Disposal_and_Media/retention_requirements.md) |

## Other regulations that sit next to HIPAA

| Citation | Topic | Module |
| --- | --- | --- |
| 42 CFR Part 2 | Substance use disorder records. Pointers: §2.11 definitions, §2.12 applicability, §2.31 consent, §2.32 redisclosure notice. Text: [eCFR](https://www.ecfr.gov/current/title-42/part-2) | [Part 2](../13_Special_Contexts/substance_use_records_part_2.md) |
| 45 CFR Part 171 | Information blocking: actors, EHI, exceptions. Text: [eCFR](https://www.ecfr.gov/current/title-45/part-171). Verify the current exception list there | [information blocking](../15_Related_Laws/information_blocking_and_interoperability.md) |
| 16 CFR Part 318 | FTC Health Breach Notification Rule (non-HIPAA apps) | [health apps](../11_Tracking_Tech_and_Health_Apps/health_apps_and_non_hipaa_data.md) |
| 45 CFR Part 102 | Annual inflation adjustment of CMP amounts | [penalties](../16_Enforcement_and_Case_Law/ocr_penalties_and_how_enforcement_works.md) |

## Try this

Open §164.312(b) on eCFR and explain it in one sentence: it requires "hardware, software, and/or procedural mechanisms that record and examine activity in information systems that contain or use ePHI". Then name the AWS service that would produce that record for PulseLedger's `pl-prod-rcm` account, and the gap that exists today (S3 data events off, application logs kept 30 days).
