# HIPAA in Practice — a learning path

A hands-on course in how HIPAA compliance is actually done at a US health-tech company and its vendors in 2026. It is written for someone in India learning **cloud security and compliance** who wants to do the job, not just pass a quiz. Many of those jobs sit at business associates, RCM and BPO firms, and SaaS vendors serving US healthcare.

- **Real law, real cases, real numbers.** Every rule cites its 45 CFR section, with real OCR resolution agreements and penalties, court decisions, and the lessons each one teaches.
- **One running example.** [PulseLedger Health](00_Start_Here/02_meet_pulseledger.md) is a Pune-founded revenue-cycle and health-messaging company. It runs on AWS, has coders in Pune and Chennai, an AI clinical-notes feature, and a consumer health app that HIPAA doesn't cover. Every module applies the rules to it.
- **Business-associate view.** Most HIPAA jobs open to you are at vendors, not hospitals, so the course covers BAAs, subcontractor chains, offshore access and the breach clocks that flow up to customers in depth.
- **Cloud-native.** Rules are translated into AWS controls, Terraform/SCP snippets, logging, deletion and evidence.
- **India lens.** Offshoring PHI to India, CERT-In's 6-hour clock, and a HIPAA vs GDPR vs DPDP comparison.
- **Each file ends with a "Try this" exercise.** Do them; together they form a portfolio.

Track your reading in [progress.md](progress.md). The GDPR companion course is at [Ayush-Labs06/GDPR_Docs](https://github.com/Ayush-Labs06/GDPR_Docs).

> Current as of **November 2026**. Rules are still moving:
> - The 2025 Security Rule proposal (NPRM) is still not final.
> - The 2024 reproductive-health Privacy Rule was vacated in 2025.
> - The 42 CFR Part 2 changes took effect on 16 February 2026.
> - State consumer-health laws keep multiplying.
>
> Files mark uncertain items "check current status". Check primary sources before relying on anything at work. This is learning material, not legal advice. PulseLedger Health and every person and customer in it are fictional.

## Learning path

| # | Module | Files | You'll be able to… |
| --- | --- | --- | --- |
| 00 | [Start here](00_Start_Here/) | 3 | Explain HIPAA's structure and enforcers, and what it is *not*; know the case company |
| 01 | [Scope and roles](01_Scope_and_Roles/) | 3 | Decide who is a covered entity or business associate; map subcontractor chains; set up the privacy and security officials |
| 02 | [PHI and data mapping](02_PHI_and_Data_Mapping/) | 3 | Tell PHI from non-PHI, map ePHI in AWS, de-identify data properly |
| 03 | [Privacy Rule](03_Privacy_Rule/) | 4 | Apply TPO, minimum necessary, authorizations and the §164.512 disclosures |
| 04 | [Notice of Privacy Practices](04_Notice_of_Privacy_Practices/) | 1 | Audit an NPP, and know a BA's role in supporting one |
| 05 | [Individual rights](05_Individual_Rights/) | 3 | Run access requests end to end; handle amendment, accounting, restrictions, and confidential communications |
| 06 | [Security Rule](06_Security_Rule/) | 3 | Read the safeguards, write a real risk analysis, map safeguards to AWS |
| 07 | [Breaches and incidents](07_Breaches_and_Incidents/) | 3 | Run the four-factor assessment, hit every notification clock, run a ransomware tabletop |
| 08 | [BAAs and vendors](08_Business_Associate_Agreements_and_Vendors/) | 2 | Review a BAA clause by clause; tier and manage vendors and cloud providers |
| 09 | [Offshore and India](09_Offshore_and_India/) | 2 | Explain what HIPAA does and doesn't say about offshoring; assess offshore coder access |
| 10 | [Retention, disposal and media](10_Retention_Disposal_and_Media/) | 2 | Get retention right (the 6-year myth) and make deletion real in the cloud |
| 11 | [Tracking tech and health apps](11_Tracking_Tech_and_Health_Apps/) | 2 | Audit pixels on health sites; handle health data outside HIPAA (FTC, Washington MHMDA) |
| 12 | [AI and health data](12_AI_and_Health_Data/) | 2 | Review an LLM feature that uses PHI; map the AI rules beyond HIPAA |
| 13 | [Special contexts](13_Special_Contexts/) | 3 | Handle Part 2 SUD records, minors and sensitive care, and employer health data |
| 14 | [Compliance program and audit](14_Compliance_Program_and_Audit/) | 4 | Run a BA's HIPAA programme; design controls and evidence; crosswalk to NIST/ISO/SOC 2/HITRUST; face OCR and customer questionnaires |
| 15 | [Related laws](15_Related_Laws/) | 3 | Compare HIPAA, GDPR and DPDP; know state laws and information blocking |
| 16 | [Enforcement and case law](16_Enforcement_and_Case_Law/) | 2 | Explain the penalty tiers and cite the cases that define the rules |
| 17 | [Career and practice](17_Career_and_Practice/) | 3 | Understand healthcare privacy/security roles, build a portfolio, prepare for interviews |
| — | [References](references/) | 2 | Find the CFR section and the primary source fast |

## How to read it

- **In order** the first time (00 → 17). Modules build on each other and on the PulseLedger story.
- **Short on time?** Read 00, 01, 02, 06, 07, 08 and 09, then do the interview questions in 17.
- **Coming from security/GRC?** Start with [Security Rule safeguards mapped to AWS](06_Security_Rule/safeguards_mapped_to_aws.md) and the [framework crosswalk](14_Compliance_Program_and_Audit/framework_crosswalk.md), so you anchor HIPAA to what you already know. Then go back to 00.
- **Did the GDPR course first?** Read [HIPAA vs GDPR vs DPDP](15_Related_Laws/hipaa_vs_gdpr_vs_dpdp.md) early. The biggest mental shift: HIPAA has no lawful-basis menu. It starts from "no use or disclosure unless permitted".
- Keep the [glossary](00_Start_Here/03_glossary.md) and the [CFR quick map](references/cfr_section_quick_map.md) open in another tab.
