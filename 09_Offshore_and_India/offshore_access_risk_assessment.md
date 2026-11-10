# Offshore access risk assessment: Pune and Chennai coders (worked example)

This is a worked assessment, shaped like a transfer impact assessment (TIA) from the GDPR world, but built for HIPAA's Security Rule. Use it as a template when a customer, a payer or your own board asks: "Is it safe to let people in India see PHI?" The legal background is in [offshoring_phi_to_india.md](./offshoring_phi_to_india.md).

Assessment owner: Priya Nair. Reviewed by Rohan Kulkarni (Security Official) and Dr. Leah Morgan (Privacy Official). Date: February 2026 (as-is) with a re-score in September 2026.

## 1. Scope in one table

| Element | PulseLedger fact |
| --- | --- |
| Data | Claims, eligibility, demographics, diagnosis and procedure codes, remittances (835), some dictation transcripts. Includes **Part 2 SUD records** from Lakeshore, mixed in (no tagging yet) |
| Purpose | Payment activity for customers: coding, scrubbing, denials, posting. This is "payment" under the Privacy Rule (§164.506) |
| Volume | ~9.2 million patients' PHI held. Coders touch a daily subset |
| People | 380 RCM operations staff and certified coders (Pune, Chennai), 150 engineers (Pune), 45 freelance coding-QA contractors via a staffing firm |
| Access path | Amazon WorkSpaces VDI in `us-east-1`; coders also use **customer-issued logins** into customer EHRs |
| Storage | AWS `us-east-1` production, `us-west-2` DR. Nothing is meant to be stored in India |
| Legal context | HIPAA via BAA chain; Indian law: DPDP Act/Rules, CERT-In Directions 2022, IT Act s.43A (status: check) |

## 2. Threats specific to offshore access

1. **Insider misuse** (curiosity, theft, bribery). INC-2026-035: a Pune coder looked up a celebrity's claim history with no work reason. A new access-anomaly alert caught it.
2. **Data leaving through the workstation**: clipboard, screenshots, phone photos, printing, USB.
3. **Credential sharing**. Four coders share one customer-issued login at a Riverbend EHR. That breaks unique user identification (§164.312(a)(2)(i)) and makes audit trails meaningless.
4. **Uncontrolled endpoints**: contractors on personal laptops for QA review.
5. **Physical exposure** of the ops floor (visitors, phones, paper).
6. **Local file stores**: the Chennai file server held claims exports and was hit by ransomware (INC-2026-031, 61,400 people).
7. **Legal compulsion** from a foreign authority (low likelihood, high impact; handled by process).
8. **Weak workforce vetting and training evidence** for contractors.

## 3. Controls table: as-is, target, evidence

| Control | As-is (Jan 2026) | Target | Evidence |
| --- | --- | --- | --- |
| VDI redirection | Clipboard and local printing **allowed** in Chennai policy | Clipboard, drive, printer, USB redirection **off**; no local storage; session watermark with user ID | WorkSpaces group policy export, screenshot of settings, quarterly config diff |
| Session control | Long idle timeouts | Auto-logoff at 15 min idle (§164.312(a)(2)(iii), addressable) | Directory policy, test record |
| Queue scoping | Coders see broad queues | Queues scoped **per client** and per coding specialty | Role-to-queue matrix; IAM Identity Center assignment report. See [minimum_necessary.md](../03_Privacy_Rule/minimum_necessary.md) |
| Unique logins | 4 coders share one Riverbend login | Ask Riverbend for named accounts, or put access behind a brokered credential vault with checkout per person and per session logging | Riverbend ticket, vault checkout logs |
| Monitoring | Weak | Access-anomaly alerts on out-of-queue and VIP lookups; weekly review (§164.308(a)(1)(ii)(D)) | Alert rules, review sign-offs, INC-2026-035 file |
| MFA | Missing on the legacy admin console | MFA everywhere, including VDI sign-in | IdP policy export |
| Background checks | Inconsistent | Pre-hire checks under Indian law with consent (identity, address, education, employment, criminal where lawful) | HR checklist, vendor report log |
| Agreements | NDAs for staff; none for contractors | NDA + HIPAA/PHI undertaking for all; **staffing-firm BAA**; intercompany BAA US Inc to India Pvt Ltd | Signed copies in the BAA register |
| Training | Annual video; no Chennai contractor records | Role-based training before access; completion gate in the IdP | LMS export |
| Physical | Phones allowed on floor | No phones/cameras on the ops floor; lockers; badge access; visitor log; clean-desk checks | Policy, floor audit photos, badge logs |
| Endpoints | Personal laptops in one QA workflow | Managed devices or VDI only; no PHI on personal devices | MDM report, VDI-only network rule |
| File servers | Chennai file server held exports | Retire it; exports go to S3 with KMS in the US account | Decommission ticket |
| Legal process | None | Written procedure: any government request goes to Marcus Webb; notify the CE unless prohibited | Procedure and request log |

Map the AWS-side controls to [safeguards_mapped_to_aws.md](../06_Security_Rule/safeguards_mapped_to_aws.md).

## 4. Scoring

Scale: likelihood (L) and impact (I) from 1 to 5. Risk = L x I. 1-6 low, 8-12 medium, 15-25 high.

| Risk | L before | I | Before | L after | After |
| --- | --- | --- | --- | --- | --- |
| Data exfiltration from a coder workstation (clipboard, print, photo) | 4 | 4 | **16 High** | 2 | 8 Medium |
| Insider snooping on VIP patients | 4 | 3 | 12 Medium | 2 | 6 Low |
| Shared customer EHR login, no accountability | 5 | 3 | **15 High** | 2 | 6 Low |
| Contractor personal laptop holds PHI | 4 | 4 | **16 High** | 1 | 4 Low |
| Ransomware on local Chennai file server | 4 | 5 | **20 High** | 1 | 5 Low |
| Foreign legal compulsion | 1 | 4 | 4 Low | 1 | 4 Low |

**Residual risk for the Chennai centre:** before = High (average driven by the exfiltration, file-server and contractor-laptop risks). After the controls are live and evidenced = Low to Medium, with the exfiltration path (photo of a screen) as the main remaining item. Screens can be photographed even when everything else is locked, so physical rules and watermarking remain the answer.

## 5. Decision

1. Offshore access **may continue** only on the target controls; it is not "approved as-is".
2. Hard conditions before signing Summit Valley's attestation: intercompany BAA signed, staffing-firm BAA signed, VDI redirection off in Chennai, shared Riverbend login ended, file server retired.
3. Re-score in 90 days. Risks that stay High go into the risk register ([risk_analysis_with_worked_example.md](../06_Security_Rule/risk_analysis_with_worked_example.md)).
4. Customers with a BAA that bans or limits offshore access: do not touch their PHI from India until written consent exists.
5. Document the decision (§164.316(b)). A decision you cannot show is a decision OCR will treat as not made.

## Try this

Rate residual risk for the Chennai centre **before and after** using the scoring table. Add two risks of your own (for example, a visitor reading a screen, or a coder using a phone camera). For each, name the control, the owner and the evidence file you would put in the audit folder.
