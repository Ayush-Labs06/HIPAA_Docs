# Risk analysis and risk management — with a full worked example (§164.308(a)(1))

If you learn one Security Rule thing well, make it this. A missing or thin risk analysis is the finding OCR cites most often, and it sits behind most big settlements.

## What the rule asks

- **Risk analysis** (§164.308(a)(1)(ii)(A), R): an accurate and thorough assessment of the potential risks and vulnerabilities to the confidentiality, integrity and availability of **all ePHI** the organization creates, receives, maintains or transmits.
- **Risk management** (§164.308(a)(1)(ii)(B), R): reduce those risks to a reasonable and appropriate level.
- **Sanction policy** (§164.308(a)(1)(ii)(C), R) and **information system activity review** (§164.308(a)(1)(ii)(D), R): punish violations; regularly review audit logs, access reports and incident reports.

"All ePHI" is the trap. A vendor questionnaire is not an analysis. A tool-by-tool scan that skips an office or a product is not an analysis.

## Method in five steps

1. **Scope**: every system, person, location and vendor that touches ePHI (see [ephi_data_mapping_in_the_cloud.md](../02_PHI_and_Data_Mapping/ephi_data_mapping_in_the_cloud.md)).
2. **Threats and vulnerabilities**: who or what could hurt it, and what weakness lets them.
3. **Current controls**: what already reduces the risk.
4. **Likelihood x impact** (1–5 each) gives a score.
5. **Treat and track**: mitigate, transfer, avoid or accept (with sign-off), each with an owner and a date.

Sources: OCR's *Guidance on Risk Analysis Requirements under the HIPAA Security Rule* (July 2010); NIST SP 800-30 Rev. 1 (risk assessment); NIST SP 800-66 Rev. 2 (February 2024, maps the Security Rule to CSF and SP 800-53). HHS also offers a free **Security Risk Assessment (SRA) Tool**, built for small providers; good for learning, thin for a cloud company.

Score bands: **15–25 High**, **8–14 Medium**, **1–7 Low**.

## PulseLedger's risk register (v1, built from the as-is state)

L = likelihood, I = impact.

| ID | Asset | Threat | Vulnerability | Existing control | L | I | Risk | Treatment | Owner | Due |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| R-01 | LedgerRCM legacy admin console | Credential theft | No MFA | Passwords | 4 | 5 | **20** | Mitigate: SSO + MFA, then retire console | Rohan | 2026-03-31 |
| R-02 | EC2 MySQL host, 2019–21 claim archives | Snapshot or volume exposure | Unencrypted EBS | VPC, SG rules | 3 | 5 | **15** | Mitigate: encrypted copy, purge old volume | Sneha | 2026-04-30 |
| R-03 | Audit logging (CloudTrail, app) | Undetected misuse or breach | S3 data events off; app logs 30 days | CloudTrail mgmt events | 4 | 4 | **16** | Mitigate: data events, 6-year archive | Rohan | 2026-04-15 |
| R-04 | `pl-dev` data, prod dictation audio | Public exposure | Real PHI in dev; weak guardrails | None | 4 | 5 | **20** | Avoid: purge, synthetic data, SCPs | Sneha | 2026-04-30 |
| R-05 | Chennai VDI sessions | Data copied out | Clipboard + local print allowed | Login only | 4 | 4 | **16** | Mitigate: redirections off, watermark | Rohan | 2026-03-15 |
| R-06 | Riverbend EHR login | Unattributable access | 4 coders share one login | Customer-issued | 5 | 4 | **20** | Mitigate: named accounts via Riverbend | Farhan | 2026-03-31 |
| R-07 | QA review by contractors | Lost or stolen device | Personal laptops; no BAA | NDA | 4 | 4 | **16** | Avoid: PHI only in VDI; BAA with firm | Marcus | 2026-04-30 |
| R-08 | App logs sent to LogPeak | Vendor misuse or breach | Names + MRNs, no BAA | TLS | 4 | 3 | **12** | Mitigate: scrub fields, BAA or replace | Rohan | 2026-05-15 |
| R-09 | ScribeAssist transcription | Vendor retention | Pilot contract, no BAA | None | 4 | 4 | **16** | Mitigate: BAA or swap before launch | Marcus | 2026-05-31 |
| R-10 | Bill-pay portal pages | Tracker disclosure to ad vendors | Meta Pixel + GA, incl. login | None | 5 | 3 | **15** | Avoid: remove trackers | Sneha | 2026-03-31 |
| R-11 | Internal chat tool | Pasted PHI | Plan has no BAA | Policy memo | 4 | 3 | **12** | Mitigate: BAA tier or DLP block | Rohan | 2026-05-31 |
| R-12 | Lakeshore Part 2 records | Over-broad access | Mixed with general data | None | 3 | 5 | **15** | Mitigate: tag and segment | Sneha | 2026-06-30 |
| R-13 | Chennai workforce | Social engineering | No training records | Annual video | 4 | 3 | **12** | Mitigate: LMS, role training | Priya | 2026-04-15 |
| R-14 | Chennai ops-centre file server | Ransomware, exfiltration | Never scoped; claims exports stored locally | Antivirus | 4 | 5 | **20** | Mitigate: move to S3 + VDI, EDR, egress alerts | Rohan | 2026-05-31 |

Notice R-14: it was found only because Chennai was finally in scope. Risk analysis is a discovery exercise. See how the register predicted later events in [ransomware_tabletop_exercise.md](../07_Breaches_and_Incidents/ransomware_tabletop_exercise.md) and [is_it_a_breach.md](../07_Breaches_and_Incidents/is_it_a_breach.md): INC-2026-024 is R-04, INC-2026-031 is R-14, INC-2026-027 is R-11.

## From register to management plan

- Sort by score. Anything **15+** gets a funded owner and date at the next leadership meeting.
- **Accept** only with a named executive signature and a review date. Never accept a Required spec silently.
- Re-run the register on any trigger: a new product, new office, new vendor, major incident, or at least yearly. ScribeAssist should have triggered one before its June 2026 launch.
- Keep each version for **6 years** (§164.316(b)(2)).

## Sanctions and log review in practice

INC-2026-035: a Pune coder viewed a local celebrity's claim history with no work reason. A new access-anomaly alert caught it. That is §164.308(a)(1)(ii)(D) working. The sanction policy (§164.308(a)(1)(ii)(C)) then needs a graded, documented outcome (retraining, suspension or termination), applied consistently and recorded. Dana Alvarez at Riverbend must also be told under the BAA, because it is an impermissible use ([breach_notification_60_days.md](../07_Breaches_and_Incidents/breach_notification_60_days.md)).

## Real enforcement

- **Anthem, US$16 million (OCR, October 2018)**: 2015 cyberattack exposing about 79 million people; OCR cited no enterprise-wide risk analysis, among other failures. Lesson: scope must be the whole enterprise.
- **Premera Blue Cross, US$6.85 million (OCR, September 2020)**: breach affecting over 10 million people; failures included risk analysis gaps. Lesson: findings repeat; OCR monitors for two years.
- **Montefiore Medical Center, US$4.75 million (OCR, February 2024)**: a malicious insider sold patient data (12,517 patients); OCR cited weak risk analysis and activity review. Lesson: it is the INC-2026-035 pattern, at scale.
- **OCR Risk Analysis Initiative**: launched in 2024. A July 2026 report counted the **14th** action under it (check current status for the latest count).

## Try this

Add three PulseWell risks (for example: unauthenticated analytics SDK in the app, long-lived API tokens, third-party ad ID sharing). Score them. Then write two sentences on why they are not HIPAA risks (PulseWell is consumer-direct, not on behalf of a provider) and which framework governs them instead (FTC Act and Health Breach Notification Rule, plus state consumer health laws; see [health_apps_and_non_hipaa_data.md](../11_Tracking_Tech_and_Health_Apps/health_apps_and_non_hipaa_data.md)).
