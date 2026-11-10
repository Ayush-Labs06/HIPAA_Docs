# Interview questions (with strong answers)

Read the question, answer out loud or on paper **before** opening the model answer. The answers are short versions of what a strong candidate says, not scripts to memorise. Several use PulseLedger facts from [Meet PulseLedger](../00_Start_Here/02_meet_pulseledger.md). Say "check current status" when a rule is in motion.

## Fundamentals

**1. What is HIPAA, and what does it not do?**
<details><summary>Answer</summary>

A 1996 US law (Pub. L. 104-191). Its Administrative Simplification part produced the Privacy Rule, Security Rule, Breach Notification Rule, Enforcement Rule and standard transactions. HITECH (2009) made business associates (BAs) directly liable and tightened penalties. What it does not do: cover all health data (only covered entities and BAs), grant a private right of action, or offer any "certification". See [What HIPAA actually is](../00_Start_Here/01_what_hipaa_actually_is.md).
</details>

**2. Covered entity or business associate: how do you decide?**
<details><summary>Answer</summary>

A covered entity (CE) is a health plan, a clearinghouse, or a provider that conducts standard electronic transactions (§160.103). A BA creates, receives, maintains or transmits PHI for a CE, or provides certain services to it. Decide by function, not by label. PulseLedger is a BA to its provider customers and is not itself a CE. See [Who HIPAA applies to](../01_Scope_and_Roles/who_hipaa_applies_to.md) and [Business associates](../01_Scope_and_Roles/business_associates_explained.md).
</details>

**3. What counts as PHI? Is an IP address on a portal login page PHI?**
<details><summary>Answer</summary>

Health information that identifies, or could identify, a person, held by a CE or BA. The 18 Safe Harbor identifiers include IP addresses. On a page where a person is logging in to a provider's portal, an IP address links a visitor to a provider relationship, which is why tracking tools there are risky. Context matters. See [What counts as PHI](../02_PHI_and_Data_Mapping/what_is_phi.md) and [Tracking technologies](../11_Tracking_Tech_and_Health_Apps/tracking_technologies.md).
</details>

**4. What is minimum necessary, and when does it not apply?**
<details><summary>Answer</summary>

Use, disclose or request only the PHI needed for the purpose (§164.502(b)). It does not apply to treatment disclosures, disclosures to the individual, authorized disclosures, disclosures to HHS, or those required by law. In practice: role-based access, client-scoped coder queues, field masking. See [Minimum necessary](../03_Privacy_Rule/minimum_necessary.md).
</details>

**5. Can HHS certify a company or product as meeting HIPAA?**
<details><summary>Answer</summary>

No. HHS does not certify organizations or tools. Vendors can show a SOC 2 report, a HITRUST assessment or an ISO 27001 certificate, which are different things. I would say "assessed against the Security Rule on this date, with these findings", never "we are compliant". See [Framework crosswalk](../14_Compliance_Program_and_Audit/framework_crosswalk.md).
</details>

## Security Rule and cloud

**6. What does "addressable" mean?**
<details><summary>Answer</summary>

Not optional. For an addressable specification you assess whether it is reasonable and appropriate. If yes, implement it. If not, document why and use an equivalent alternative or nothing if truly unnecessary (§164.306(d)). The decision must be written down. A 2025 proposal would remove the distinction; check current status. See [Security Rule explained](../06_Security_Rule/security_rule_explained.md).
</details>

**7. Why is the risk analysis such a big deal?**
<details><summary>Answer</summary>

It is repeatedly cited in OCR enforcement, and investigators commonly ask for it early. I keep it current, scoped to all ePHI, and tied to a management plan. See [Risk analysis](../06_Security_Rule/risk_analysis_with_worked_example.md) and [OCR penalties](../16_Enforcement_and_Case_Law/ocr_penalties_and_how_enforcement_works.md).
</details>

**8. How does the AWS BAA change your obligations?**
<details><summary>Answer</summary>

It lets you put ePHI in HIPAA-eligible services. It does not make your workload compliant. Under shared responsibility you still configure encryption, access, logging, backups and retention, and you must check each service you use against the eligible list. I would say "HIPAA-eligible services under a BAA". See [Safeguards mapped to AWS](../06_Security_Rule/safeguards_mapped_to_aws.md).
</details>

**9. How would you stop the next public S3 bucket?**
<details><summary>Answer</summary>

Layered: account-level Block Public Access, an SCP denying changes to it, AWS Config rules with auto-remediation, tagging data classes, Macie for discovery, and logging so access can be proven. That is what would have prevented or narrowed INC-2026-024. See [Safeguards mapped to AWS](../06_Security_Rule/safeguards_mapped_to_aws.md).
</details>

**10. Does encryption mean no breach notification?**
<details><summary>Answer</summary>

Only if the data meets HHS guidance for "secured" PHI (valid encryption, keys not compromised). Ransomware that encrypts files at rest in a victim's systems does not count; OCR's ransomware fact sheet presumes a breach unless low probability of compromise is shown. See [Is it a breach?](../07_Breaches_and_Incidents/is_it_a_breach.md).
</details>

## Breach and incidents

**11. Define a breach under HIPAA.**
<details><summary>Answer</summary>

An impermissible use or disclosure of unsecured PHI is presumed a breach unless a risk assessment shows a low probability of compromise (§164.402). Three exceptions exist: unintentional good-faith access by a workforce member, inadvertent disclosure between authorized people at the same entity, and a recipient who could not have retained it.
</details>

**12. Name the four factors.**
<details><summary>Answer</summary>

(1) The nature and extent of the PHI, including identifiers and likelihood of re-identification. (2) Who used or received it. (3) Whether it was actually acquired or viewed. (4) How far the risk has been mitigated. Document the outcome either way. See [Is it a breach?](../07_Breaches_and_Incidents/is_it_a_breach.md).
</details>

**13. INC-2026-019: a coder emailed 412 patients' remittances to the wrong practice, which confirmed deletion in writing. Breach?**
<details><summary>Answer</summary>

Arguable both ways. Low probability: one known recipient, a covered entity bound by HIPAA, written deletion assurance, quick. Against: financial and diagnosis-adjacent data, 412 people. I would document the four-factor memo and let the CE customer decide, since the CE owns the notification duty. Either way log it for the annual HHS report if it is a breach (under 500).
</details>

**14. INC-2026-024: a dev bucket with production audio was public for 31 hours and server access logs were off. Breach?**
<details><summary>Answer</summary>

Treat it as a breach. Without logs I cannot show who accessed it, so I cannot show low probability of compromise, and the burden of proof is on the entity (§164.414). 1,850 patients of three Riverbend clinics. Notify Riverbend promptly under the BAA, support its notices, and fix root causes: dev data copies, public access, missing logs.
</details>

**15. Who notifies whom, and by when?**
<details><summary>Answer</summary>

BA to CE without unreasonable delay and no later than 60 days from discovery (§164.410), shorter if the BAA says so. CE to individuals within 60 days (§164.404). CE to HHS: 500 or more, within 60 days; under 500, within 60 days after the calendar year ends. Media notice for more than 500 residents of a state. Other clocks stack: state laws, CERT-In 6 hours in India, customer BAAs. See [Breach notification](../07_Breaches_and_Incidents/breach_notification_60_days.md).
</details>

**16. INC-2026-031: ransomware on the Chennai file server, 4.2 GB exfiltrated, 61,400 people including Part 2 records. First 24 hours?**
<details><summary>Answer</summary>

Contain and preserve evidence; engage counsel and forensics; start the clocks. Notify customers under their BAAs (Riverbend 24 hours, Lakeshore 5 days in this case), report to CERT-In within 6 hours of noticing, and tell law enforcement if appropriate. Presume breach because of exfiltration. Part 2 records add rules; check current status of the 2024 final rule's breach alignment. See [Ransomware tabletop](../07_Breaches_and_Incidents/ransomware_tabletop_exercise.md).
</details>

## Scenarios, BAA and offshore

**17. Is PulseWell, our direct-to-consumer symptom app, covered by HIPAA?**
<details><summary>Answer</summary>

Not on these facts. Consumers sign up themselves and it is not offered on behalf of a provider, so PulseLedger is not a CE or BA for it. Other rules apply: FTC Act, the FTC Health Breach Notification Rule, and state laws such as Washington's My Health My Data Act. If a provider later pays for or integrates it, the answer could change. See [Health apps outside HIPAA](../11_Tracking_Tech_and_Health_Apps/health_apps_and_non_hipaa_data.md).
</details>

**18. Four coders share one Riverbend EHR login. What do you do?**
<details><summary>Answer</summary>

Stop it. Shared logins break unique user identification (§164.312(a)(2)(i)) and make audit logs useless, and it breaches the customer's terms. Raise it with Farhan Sheikh and the customer's privacy officer, get individual accounts, record the exception and a deadline, and add a risk-register entry. Without individual accounts, snooping like INC-2026-035 is untraceable.
</details>

**19. A patient emails PulseLedger asking for "all my records". What happens?**
<details><summary>Answer</summary>

PulseLedger is not the CE, so it does not decide the request. Log it, verify nothing beyond routing, tell the patient politely that their provider handles it, and forward to the CE contact within the BAA timeframe. Never send PHI to an unverified email.
</details>

**20. Sales wants to send a pharma-sponsored message through PulseConnect. Your view?**
<details><summary>Answer</summary>

That is marketing; it needs a patient authorization if the CE receives payment from the third party, and the BAA likely does not permit it. I would stop it until legal and the customer agree. See [Authorizations](../03_Privacy_Rule/authorizations_and_special_permissions.md).
</details>

**21. Can ScribeAssist fine-tune models on customer PHI?**
<details><summary>Answer</summary>

Only if the BAA permits it for that purpose and the CE agrees. "Improving our models" is usually not TPO for the customer. Safer designs: de-identify first, use opt-in customers with a BAA amendment, or train on synthetic data. Check the LLM vendor's BAA and retention terms. See [AI with PHI](../12_AI_and_Health_Data/ai_with_phi.md).
</details>

**22. Summit Valley Health Plan sends a 280-question questionnaire and an offshore attestation. How do you answer?**
<details><summary>Answer</summary>

Build an answer library from evidence, not from marketing. Answer what is true today, state gaps with dates, and avoid "HIPAA compliant". For offshore, explain the BAA chain, VDI controls, access scoping and Indian-law layers. The CMS rules for Medicare Advantage offshore subcontractors are specific; check current status before attesting. See [Offshoring PHI to India](../09_Offshore_and_India/offshoring_phi_to_india.md).
</details>

**23. A vendor holds PHI and has no BAA. What do you do?**
<details><summary>Answer</summary>

Find out what flows (for LogPeak, names and MRNs in logs). Stop or reduce the flow if there is no BAA, sign one, or move to a vendor that will. Treat the past flow as a possible impermissible disclosure and assess it. Track in the vendor register. See [Vendor management](../08_Business_Associate_Agreements_and_Vendors/vendor_and_cloud_provider_management.md).
</details>

## Try this

Pick 5 questions from different groups. Set a timer for 90 seconds each and record yourself answering aloud. Play it back and mark: did you start with the rule, apply it to a fact, and say what you would do? Rewrite any answer where you said "it depends" without saying on what.
