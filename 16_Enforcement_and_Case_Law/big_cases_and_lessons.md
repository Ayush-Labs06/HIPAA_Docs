# Big cases and what each one teaches

Every case below was checked against an HHS, court or state-AG source in November 2026. Where only a secondary source was available, the row says so. Cases I could not confirm were dropped. Amounts are what was paid or ordered, not what was demanded. Settlements carry "no admission of liability".

## The table

| # | Case | Year | Amount | What went wrong | Lesson | Module |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | Anthem (OCR) | 2018 | **US$16 million** | Hackers took data of almost 79 million people (2014-15). No enterprise-wide risk analysis, weak monitoring | Largest OCR payment at the time; risk analysis again | [risk analysis](../06_Security_Rule/risk_analysis_with_worked_example.md) |
| 2 | Anthem (state AGs) | 2020 | **US$39.5 million** | Same breach; a coalition of 43-44 states (state releases differ on the count) | AGs enforce too, with security-program terms | [state laws](../15_Related_Laws/state_health_privacy_laws.md) |
| 3 | Premera Blue Cross | 2020 | **US$6.85 million** | Attack on 10.4 million people; no enterprise-wide risk analysis, weak risk management and audit controls | OCR's second-largest at that time | [risk analysis](../06_Security_Rule/risk_analysis_with_worked_example.md) |
| 4 | Banner Health | 2023 | **US$1.25 million** | 2016 hack, 2.81 million people; no organization-wide risk analysis, weak activity review and authentication | Authentication and log review are tested | [safeguards on AWS](../06_Security_Rule/safeguards_mapped_to_aws.md) |
| 5 | MedEvolve (BA) | 2023 | **US$350,000** | Unsecured FTP server open since 1 Jan 2018; 230,572 people; no BAA with a subcontractor; no risk analysis | A BA pays directly, and the subcontractor chain matters | [BAA review](../08_Business_Associate_Agreements_and_Vendors/baa_clause_by_clause_review.md) |
| 6 | Montefiore Medical Center | 2024 | **US$4.75 million** | Insider stole and sold data of 12,517 patients; police flagged it in 2015; OCR cited no risk assessment and no activity review | Log review catches insiders; settled nine years later | [risk analysis](../06_Security_Rule/risk_analysis_with_worked_example.md) |
| 7 | Bryan County Ambulance Authority | 2024 | **US$90,000** | 2022 ransomware, 14,273 people; first action under the Risk Analysis Initiative (31 Oct 2024) | Small entities are not exempt | [ransomware tabletop](../07_Breaches_and_Incidents/ransomware_tabletop_exercise.md) |
| 8 | Warby Parker | 2025 | **US$1.5 million CMP** | Cyberattack, nearly 200,000 people; no accurate and thorough risk analysis, weak safeguards and log review. OCR proposed the penalty Sept 2024; Warby Parker waived a hearing; final Dec 2024; announced 20 Feb 2025 | OCR does impose CMPs when no deal is reached | [penalties](ocr_penalties_and_how_enforcement_works.md) |
| 9 | Comstar (BA) | 2025 | **US$75,000** | Ambulance billing firm; March 2022 ransomware; 585,621 people; no risk analysis; BA to 70+ entities | A billing/RCM vendor is the closest match to PulseLedger | [BAs](../01_Scope_and_Roles/business_associates_explained.md) |
| 10 | Four ransomware cases | Apr 2026 | **US$1.165 million** in total | OCR announced four settlements on 23 Apr 2026 covering 427,000+ people; same finding in each: no accurate and thorough risk analysis | OCR's own count after this: 19 ransomware, 13 risk-analysis cases | [risk analysis](../06_Security_Rule/risk_analysis_with_worked_example.md) |
| 11 | Bayfront Health St. Petersburg | 2019 | **US$85,000** | First Right of Access action: a mother waited over nine months for records | Start of the access initiative | [right of access](../05_Individual_Rights/right_of_access.md) |
| 12 | Azul Vision | Aug 2026 | **US$50,000** (HIPAA Journal, Paubox; I did not see the HHS figure) | 55th Right of Access action, announced 27 Aug 2026; the one before it was Concentra at US$112,500 | Access cases are routine now | [right of access](../05_Individual_Rights/right_of_access.md) |
| 13 | New England Dermatology and Laser Center | 2022 | **US$300,640** | Specimen containers with patient labels thrown in a parking-lot dumpster (practice ran 2011-2021) | Labels are PHI; destroy them | [disposal](../10_Retention_Disposal_and_Media/disposal_and_media_reuse.md) |
| 14 | *MD Anderson v. HHS* | 2021 | US$4.348 million **vacated** | 5th Cir. found HHS arbitrary; "mechanism" for encryption was enough | OCR penalties can be beaten in court | [penalties](ocr_penalties_and_how_enforcement_works.md) |
| 15 | *In re Advocate Aurora Health Pixel Litigation* (private class action) | 2024 | **US$12.225 million** fund | Meta Pixel and Google Analytics on website, portal and scheduling app; final approval 10 July 2024; no ruling on the merits | Tracking tech is costly even without OCR | [tracking tech](../11_Tracking_Tech_and_Health_Apps/tracking_technologies.md) |

**Not in the table, by decision.** Change Healthcare (2024): OCR opened an investigation in March 2024, and the breach portal figure reported in the press is **192.7 million** individuals (filed 31 July 2025). I found no public OCR resolution, so there is no outcome to cite: **check current status**. Presence Health and North Memorial are cited in [is_it_a_breach.md](../07_Breaches_and_Incidents/is_it_a_breach.md) and not repeated here.

## The top six, in a few lines each

**1. Anthem, 2018 (US$16 million).** State AG releases say a phishing email let malware in; OCR described an advanced persistent threat that stole data between December 2014 and January 2015. OCR said the failures were governance failures: no enterprise-wide risk analysis, insufficient procedures to review system activity, and weak access controls. The lesson is that a big breach turns a paper gap into a multi-million fine. Source: HHS Anthem resolution page.

**2. Premera, 2020 (US$6.85 million).** OCR found "systemic noncompliance": no enterprise-wide risk analysis, no adequate risk management, weak audit controls. The CAP ran two years of monitoring. A company that treats a questionnaire as a risk analysis (PulseLedger's 2021 gap) is on this path.

**3. Montefiore, 2024 (US$4.75 million).** The crime was one employee. The failure was that nobody could see it for years. Compare INC-2026-035 in [risk_analysis_with_worked_example.md](../06_Security_Rule/risk_analysis_with_worked_example.md): PulseLedger caught its snooper only because a new access-anomaly alert existed. Information-system activity review (§164.308(a)(1)(ii)(D)) is what OCR measures.

**4. MedEvolve, 2023 (US$350,000).** A BA left a file-transfer server open for years, and had no BAA with its own subcontractor. This is PulseLedger's staffing-firm and LogPeak gap in the real world. A BA is liable even if its customers did nothing wrong.

**5. Warby Parker, 2025 (US$1.5 million).** An example of the full path in the diagram in [ocr_penalties_and_how_enforcement_works.md](ocr_penalties_and_how_enforcement_works.md): proposed determination, waived hearing, final penalty. It also shows OCR can pursue a company outside the classic hospital or insurer mould.

**6. Comstar, 2025 (US$75,000).** Small amount, large lesson: 585,621 people and 70+ customers behind one vendor. When a billing vendor is breached, every customer files its own breach report.

## Try this

For three cases, name the PulseLedger as-is gap that matches (see [meet_pulseledger.md](../00_Start_Here/02_meet_pulseledger.md)). Suggested pairs: Warby Parker or Premera with the 2021 vendor-questionnaire "risk analysis"; MedEvolve with the missing staffing-firm and LogPeak BAAs; Montefiore with the shared Riverbend login and the 30-day application audit logs; Advocate Aurora with the Meta Pixel on `pay.pulseconnect.example`. For each, write one sentence on the fix and one on the evidence you would show OCR.
