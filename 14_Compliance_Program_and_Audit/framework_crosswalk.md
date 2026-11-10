# Framework crosswalk: HIPAA ↔ NIST CSF 2.0 ↔ SP 800-53 ↔ ISO 27001:2022 ↔ SOC 2 ↔ HITRUST

You will rarely be asked for "HIPAA" on its own. A customer wants a SOC 2 report. A payer wants HITRUST. A European partner wants ISO 27001. A crosswalk lets you do the work once and show it many times. It also shows you where the other frameworks stop and HIPAA keeps going.

**Read this table as indicative.** Frameworks map imperfectly. For the authoritative HIPAA-to-NIST mapping use NIST SP 800-66 Rev. 2 (published February 2024, jointly with HHS OCR), which maps Security Rule standards to CSF subcategories and SP 800-53 Rev. 5 controls. Where the mapping now lives (appendix or NIST's online reference tool): check current status on the NIST page.

## The crosswalk (20 rows)

| # | HIPAA Security Rule standard | NIST CSF 2.0 | SP 800-53 r5 | ISO 27001:2022 | SOC 2 (TSC) | HITRUST CSF category |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | Risk analysis §164.308(a)(1)(ii)(A) | ID.RA | RA-3 | Clause 6.1.2, 8.2 | CC3.2 | 03 Risk Management |
| 2 | Risk management §164.308(a)(1)(ii)(B) | GV.RM, ID.RA | RA-7, PM-9 | Clause 6.1.3, 8.3 | CC3.4, CC9.1 | 03 Risk Management |
| 3 | Sanction policy §164.308(a)(1)(ii)(C) | GV.RR | PS-8 | A.6.4 | CC1.5 | 02 Human Resources |
| 4 | Information system activity review §164.308(a)(1)(ii)(D) | DE.CM | AU-6 | A.8.15, A.8.16 | CC7.2 | 09 Communications and Operations |
| 5 | Assigned security responsibility §164.308(a)(2) | GV.RR | PM-2 | A.5.2 | CC1.3 | 00 Security Management Program |
| 6 | Workforce security §164.308(a)(3) | PR.AA | PS-3, PS-4, AC-2 | A.6.1, A.6.5 | CC6.2, CC6.3 | 02 Human Resources |
| 7 | Information access management §164.308(a)(4) | PR.AA | AC-2, AC-5, AC-6 | A.5.15, A.5.18 | CC6.1–CC6.3 | 01 Access Control |
| 8 | Awareness and training §164.308(a)(5) | PR.AT | AT-2, AT-3 | A.6.3 | CC1.4 | 02 Human Resources |
| 9 | Security incident procedures §164.308(a)(6) | RS.MA, RS.AN, RS.CO | IR-4, IR-6, IR-8 | A.5.24–A.5.27 | CC7.3–CC7.5 | 11 Incident Management |
| 10 | Contingency plan §164.308(a)(7) | RC.RP, PR.IR | CP-2, CP-9, CP-10 | A.5.29, A.5.30, A.8.13 | A1.2, A1.3 | 12 Business Continuity |
| 11 | Evaluation §164.308(a)(8) | ID.IM | CA-2, CA-7 | Clause 9.2, 9.3; A.5.35 | CC4.1 | 06 Compliance |
| 12 | BA contracts §164.308(b), §164.314(a) | GV.SC | SA-9, SR-3 | A.5.19–A.5.22 | CC9.2 | 05 Organization of Security |
| 13 | Facility access controls §164.310(a) | PR.AA | PE-2, PE-3 | A.7.1–A.7.4 | CC6.4 | 08 Physical and Environmental |
| 14 | Workstation use and security §164.310(b)–(c) | PR.PS | AC-11, PE-5 | A.7.7, A.8.1 | CC6.4 | 08 Physical and Environmental |
| 15 | Device and media controls §164.310(d) | PR.DS | MP-4, MP-6 | A.7.10, A.7.14 | CC6.5 | 07 Asset Management |
| 16 | Access control §164.312(a) | PR.AA | AC-2, AC-3, AC-12 | A.5.15, A.8.2, A.8.3 | CC6.1 | 01 Access Control |
| 17 | Audit controls §164.312(b) | DE.CM | AU-2, AU-12 | A.8.15 | CC7.2 | 09 Communications and Operations |
| 18 | Integrity §164.312(c) | PR.DS | SI-7, SC-28 | A.8.24, A.8.13 | CC6.1, PI1.x | 09 Communications and Operations |
| 19 | Authentication and transmission security §164.312(d)–(e) | PR.AA, PR.DS | IA-2, IA-5, SC-8 | A.5.17, A.8.5, A.8.24 | CC6.1, CC6.7 | 01 Access Control; 09 Comms |
| 20 | Policies and documentation §164.316 | GV.PO | PL-1 and each family's -1 control | Clause 7.5; A.5.1 | CC5.3 | 04 Security Policy |

Check specific control numbers against the current text of each framework before putting them in a customer-facing document.

## HITRUST: what it certifies

HITRUST publishes a control framework (the CSF) and runs assessments against it. As HITRUST describes them (check its site for current details):

| Level | Idea | Typical validity |
| --- | --- | --- |
| **e1** | Essentials, about 44 requirements | 1 year |
| **i1** | Implemented controls, about 182 requirements | 1 year |
| **r2** | Risk-based, tailored, the most thorough | 2 years, with an interim review |

A HITRUST report certifies that you **met HITRUST's assessment**, not that you "comply with HIPAA". There is no government-run HIPAA certification, and no one can sell it. HHS does not endorse any framework. A good HITRUST r2 shows strong security governance. It does not test the Privacy Rule, patient rights or your BAAs. PulseLedger's roadmap lists SOC 2 and HITRUST; answer customers with the real scope of each.

**SOC 2 plus HIPAA.** A SOC 2 Type II report tests controls against the AICPA Trust Services Criteria over a period. Auditors can add a HIPAA mapping ("SOC 2+ HIPAA"). Useful, but the opinion is still on the SOC 2 criteria you chose.

## Healthcare-specific guidance

- **HHS 405(d) HICP** (Health Industry Cybersecurity Practices): a practical set of practices against the five main threats. The current edition is 2023 (check for newer).
- **HPH Cybersecurity Performance Goals (CPGs)**, published by HHS in January 2024: 10 *essential* and 10 *enhanced* goals. Voluntary, but they mirror what the 2025 Security Rule proposal would make mandatory. Status of the proposal: check the Federal Register and the regulatory agenda; reports in mid-2026 said final action had slipped to July 2027. See [../06_Security_Rule/security_rule_explained.md](../06_Security_Rule/security_rule_explained.md).
- Both are named in the recognized-security-practices law; see [./controls_and_evidence.md](./controls_and_evidence.md).

## Where the frameworks stop and HIPAA continues

| HIPAA area | Covered by ISO / SOC 2 / HITRUST? |
| --- | --- |
| Privacy Rule: uses and disclosures, minimum necessary, authorizations | Not by ISO or SOC 2 security criteria. HITRUST has a privacy category but not the legal analysis |
| Individual rights: access, amendment, accounting | No |
| Breach Notification Rule: the four-factor test and the 60-day clocks | Incident response is covered; the legal thresholds are not |
| BAA content (§164.504(e)) | Vendor management is covered; required clauses are not |
| Notice of Privacy Practices | No |
| Minimum documentation retention (6 years) | Partly |
| 42 CFR Part 2 and state laws | No |

So: a clean SOC 2 or HITRUST does not answer "do you comply with HIPAA?". Say what the report covers and show the rest through your own program. See [./ocr_investigations_audits_and_questionnaires.md](./ocr_investigations_audits_and_questionnaires.md).

## Try this

Pick five rows from the crosswalk. For each, write one sentence of evidence from your current employer, a lab or a portfolio project (for example: "AWS Config rule `encrypted-volumes` history = row 18/16"). Mark any row where you have nothing. That gap list is your study plan. Use the evidence format from [./controls_and_evidence.md](./controls_and_evidence.md).
