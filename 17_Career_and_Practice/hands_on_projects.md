# Hands-on projects (build a portfolio while you learn)

Reading will not get you hired or win a freelance client. A folder of finished, honest work will. These 8 projects each produce one artifact you can show. They all use PulseLedger Health or a fictional clinic you invent.

> **Never use real PHI.** Not your own records, not a friend's, not a "just for testing" export. Use synthetic data (for example, output from the open-source Synthea generator), invented names, and an AWS sandbox with no real patient data. When you audit a public website, observe only: load pages as a normal visitor, never log in with someone else's account, never probe or attack.

## Rules for every project

- Put the project in its own folder with a short README: goal, scope, method, what you did **not** do.
- Date everything. Law and guidance move; "as of November 2026" belongs in the file.
- Say "assessed against the HIPAA Security Rule" rather than "HIPAA compliant". A portfolio piece should never claim a fictional or real organization is compliant.
- Keep personal plans and notes out of the public repo.

## 1. Risk analysis for a fictional clinic

- **Goal:** produce a defensible risk analysis for a 4-doctor clinic you invent.
- **Steps:** (1) define scope: every system holding ePHI, including laptops, email, the EHR, the billing vendor and backups; (2) build an inventory; (3) list threats and vulnerabilities; (4) score likelihood and impact; (5) write treatments, owners and dates; (6) add a short management plan.
- **Deliverable:** a risk register with at least 15 rows, plus a 2-page summary.
- **Draws on:** [Risk analysis with worked example](../06_Security_Rule/risk_analysis_with_worked_example.md), [ePHI data mapping](../02_PHI_and_Data_Mapping/ephi_data_mapping_in_the_cloud.md).
- **Stretch:** try the HHS Security Risk Assessment Tool and compare its output to yours.

## 2. Terraform baseline landing zone (sandbox, no PHI)

- **Goal:** show you can turn Security Rule safeguards into code.
- **Steps:** in a personal sandbox AWS account or an AWS Organizations test OU, write Terraform for: an SCP denying public S3 and unencrypted EBS; account-level S3 Block Public Access; a KMS key with rotation; a multi-region CloudTrail with S3 data events on one bucket, delivered to a separate log account with Object Lock. Run `terraform plan` and keep the output. Destroy everything afterwards.
- **Deliverable:** a repo with modules, a README mapping each resource to a Security Rule standard, and a short evidence note (screenshots or CLI output with account IDs removed).
- **Draws on:** [Safeguards mapped to AWS](../06_Security_Rule/safeguards_mapped_to_aws.md), [Disposal and media reuse](../10_Retention_Disposal_and_Media/disposal_and_media_reuse.md).
- **Warn:** stay inside free-tier or low-cost services, set a billing alarm first, and say in the README that using HIPAA-eligible services under a BAA is not the same as being compliant; the configuration is on you.

## 3. BAA clause tracker

- **Goal:** compare many agreements without reading each one every time.
- **Steps:** take 3 public sample BAAs (HHS publishes sample BAA provisions; find the current page yourself and cite it). Build a sheet with one row per clause: breach-notice clock, subcontractor flow-down, return or destroy, audit rights, offshore terms, indemnity. Flag red flags. Add PulseLedger's conflicting clocks (24 hours, 5 days, 10 days, "without unreasonable delay") and pick the one you would operate to.
- **Deliverable:** the tracker plus a one-page "operating standard" memo.
- **Draws on:** [BAA clause-by-clause review](../08_Business_Associate_Agreements_and_Vendors/baa_clause_by_clause_review.md), [Business associates explained](../01_Scope_and_Roles/business_associates_explained.md).

## 4. Safe Harbor de-identification script on synthetic data

- **Goal:** apply the 18 identifiers rule in code and show where it gets hard.
- **Steps:** generate synthetic patient records with Synthea. Write a Python script that removes or generalizes direct identifiers, reduces dates to year, handles ZIP codes under the ZIP-3 rule, and flags free-text fields for human review. Test it with a record where a name hides in a note. Write down what the script cannot catch.
- **Deliverable:** script, 3 tests, and a "limits" page. Include a note that pseudonymized is not de-identified.
- **Draws on:** [De-identification and limited data sets](../02_PHI_and_Data_Mapping/de_identification_and_limited_data_sets.md), [What is PHI](../02_PHI_and_Data_Mapping/what_is_phi.md), [AI with PHI](../12_AI_and_Health_Data/ai_with_phi.md).

## 5. The ransomware tabletop

- **Goal:** facilitate an incident exercise and score it.
- **Steps:** run the INC-2026-031 tabletop with friends or play each role yourself. Record decisions at each inject, the clocks you started, and where people disagreed. Fill in the scorecard.
- **Deliverable:** a 3-page after-action report: timeline, decisions, gaps, and fixes mapped to risk-register IDs.
- **Draws on:** [Ransomware tabletop exercise](../07_Breaches_and_Incidents/ransomware_tabletop_exercise.md), [Breach notification](../07_Breaches_and_Incidents/breach_notification_60_days.md).

## 6. Tracking-technology audit of a public site (observe only)

- **Goal:** document what a health-related site sends to third parties.
- **Steps:** pick a public health-related site you may legally view. Open browser developer tools, load the pages as a visitor, save a HAR file, and list every third-party request. Mark tags that appear on pages that look authenticated or related to symptoms or appointments. Do **not** create accounts with real data, bypass logins or test beyond what a normal visitor sees. Do not publish a name-and-shame post; write the method against a fictional site, or ask the owner before naming them.
- **Deliverable:** a tag inventory, a method page, and a fix list.
- **Draws on:** [Tracking technologies](../11_Tracking_Tech_and_Health_Apps/tracking_technologies.md), [Health apps outside HIPAA](../11_Tracking_Tech_and_Health_Apps/health_apps_and_non_hipaa_data.md).

## 7. Vendor-tiering spreadsheet

- **Goal:** show a repeatable vendor-risk process.
- **Steps:** list PulseLedger's vendors from [Meet PulseLedger](../00_Start_Here/02_meet_pulseledger.md). Add columns: PHI touched, volume, BAA status, evidence held (SOC 2 Type II, HITRUST), tier, next action, due date. Tier the four no-BAA vendors first and write the remediation plan for each.
- **Deliverable:** the spreadsheet plus a one-page intake questionnaire.
- **Draws on:** [Vendor and cloud provider management](../08_Business_Associate_Agreements_and_Vendors/vendor_and_cloud_provider_management.md).

## 8. HIPAA to ISO 27001 crosswalk

- **Goal:** let a company with an ISO 27001 program see what HIPAA adds.
- **Steps:** take 20 Security Rule standards and map each to ISO 27001:2022 Annex A controls. Mark where ISO covers it, partly covers it, or does not. Add rows for what ISO does not cover: Privacy Rule, breach notification and BAAs.
- **Deliverable:** a table plus a half-page summary of gaps.
- **Draws on:** [Framework crosswalk](../14_Compliance_Program_and_Audit/framework_crosswalk.md), [Security Rule explained](../06_Security_Rule/security_rule_explained.md).

## How to present the portfolio

```
repo/
  README.md              who you are, what each project shows
  01-risk-analysis/
  02-terraform-baseline/
  03-baa-tracker/
  ...
  NOTES-local/           gitignored: plans, drafts
```

For each project, write three lines in the README: the problem, what you built, what you learned. Interviewers read short, specific lines. Link to the work, not to certificates.

## Try this

Pick two projects, one that is documentation-heavy (1, 3, 7 or 8) and one that is hands-on (2, 4, 5 or 6). Write dates in a calendar: start, midpoint check, finish. Aim for two weeks each. At the end, add each to your portfolio README and ask one person to review it and tell you where it was unclear.
