# Offshoring PHI to India: what HIPAA does and doesn't say

If you work for an Indian RCM or BPO vendor, this is the question every US customer asks first: "Can people in India see our patients' data?" The honest answer has two layers. HIPAA itself says yes, with conditions. Customers, payers and some government programmes then say no, or only with extra steps.

## What HIPAA says about location

**Nothing.** There is no data-localisation rule in the Privacy, Security or Breach Notification Rules (45 CFR Parts 160 and 164). PHI can be accessed from any country. What HIPAA cares about is the *chain of agreements and safeguards*:

- Every covered entity (CE) that shares PHI with a vendor needs a business associate agreement (BAA) (§164.502(e), §164.504(e)).
- The business associate (BA) must pass the same duties to each subcontractor (§164.502(e)(1)(ii), §164.308(b)(2)). See [business_associates_explained.md](../01_Scope_and_Roles/business_associates_explained.md).
- The Security Rule safeguards apply the same way in Pune as in Nashville (§164.306).
- HIPAA reaches the Indian entity **through the contract and through direct BA liability**. OCR has no office in Pune, but the US BA is liable for its subcontractor, and the US CE is the one OCR knocks on first.

So "HIPAA allows offshoring" is true but incomplete. The real limits come from elsewhere.

## The limits that actually bite (verify each before relying on it)

| Source | What it does | Status note |
| --- | --- | --- |
| **Customer BAA or MSA** | Many CEs ban offshore access, or require written consent, or limit it to named roles | The most common limit. Read every BAA (see [baa_clause_by_clause_review.md](../08_Business_Associate_Agreements_and_Vendors/baa_clause_by_clause_review.md)) |
| **CMS: Medicare Advantage and Part D** | Sponsors that use offshore subcontractors who handle PHI must report them to CMS and attest that PHI is protected. CMS launched an Offshore Subcontract Data module in HPMS through a memo of 26 August 2008 | Many plans still use attestation forms based on this in 2024-2025. I did not find a current CMS page restating the exact terms; **check current status** in the CMS Medicare Managed Care Manual and HPMS memos |
| **State Medicaid contracts** | Some state Medicaid agencies restrict offshore handling in their managed-care contracts | **Check current status** per state and per contract. No single rule |
| **Federal contracts (VA, DoD, TRICARE)** | Can prohibit foreign access to health data by contract clause | **Check current status** per contract |
| **Payer vendor-security terms** | Large payers often require pre-approval of offshore subcontractors, audit rights and attestations | Contractual, negotiable |

CMS and plans do not forbid India. They want to *know*, to see safeguards, and to be able to audit.

## Indian law, alongside HIPAA

A Pune company is bound by Indian law whatever the US contract says. Three layers matter:

**1. DPDP Act 2023 and DPDP Rules 2025.** MeitY notified the Rules on **13 November 2025** with phased dates. As of November 2026, the main obligations are expected about **13 May 2027** (18 months after notification; one source gives 12 May, so check the Gazette). Two points for PulseLedger:

- Section 17(1)(d), as summarised by Indian law firms, exempts processing of personal data of people **outside India** by an Indian entity under a contract with a foreign person from the core duties. This is the outsourcing carve-out. I could not confirm the exact wording from primary text: **check current status** against the Act.
- The carve-out does not cover PulseLedger's **own Indian employees** or any Indian residents' data. It also does not remove HIPAA or contract duties. Employees' data is covered in [../13_Special_Contexts/employers_and_group_health_plans.md](../13_Special_Contexts/employers_and_group_health_plans.md); a side-by-side view is in [hipaa_vs_gdpr_vs_dpdp.md](../15_Related_Laws/hipaa_vs_gdpr_vs_dpdp.md).

**2. CERT-In Directions of 28 April 2022** (under s.70B(6) of the IT Act, in force 27 June 2022):

- Report listed cyber incidents to CERT-In within **6 hours** of noticing them.
- Keep ICT system logs for a rolling **180 days**, "within the Indian jurisdiction".
- That second point matters. PulseLedger's Pune and Chennai endpoints, firewalls and VDI clients produce logs. Logs for those Indian systems should be held in India, even though the PHI and its application logs sit in `us-east-1`. Ask counsel how this applies to logs of systems that live in AWS US regions.

**3. IT Act s.43A and the SPDI Rules 2011.** These are the old Indian data-security regime. Section 44(2) of the DPDP Act omits s.43A. Secondary sources date that omission to the May 2027 phase, but one disagrees. **Check current status**. Until then, treat s.43A as still alive.

## The legal-access asymmetry

US customers ask: "Can the Indian government compel your engineers to hand over our patients' data?" In practice they are comparing two things:

- **US side:** the CE can be subpoenaed or served a warrant, and the BA can too (see the §164.512 list in [public_interest_disclosures.md](../03_Privacy_Rule/public_interest_disclosures.md)).
- **Indian side:** Indian authorities can issue directions under the IT Act (for example s.69 interception and decryption powers), and CERT-In can demand logs and information.

Your honest answer: such powers exist; here is our process (route to counsel, notify the CE where the law allows, give the minimum), and here is how we limit exposure (data stays in `us-east-1`, accessed only through VDI, nothing stored locally in India). That answer is stronger than "it never happens".

## PulseLedger: the Summit Valley attestation answers

Summit Valley Health Plan (Arizona, Medicare Advantage, a *prospect*) sent a CMS offshore-subcontractor attestation request. Jason Pike wants to close. Priya's draft answers, which Dr. Morgan and Marcus Webb must sign off:

| Likely question | Draft answer (honest, as of the as-is state) |
| --- | --- |
| Do you use offshore subcontractors who access our PHI? | Yes. PulseLedger Technologies Pvt Ltd (Pune, Chennai) is a group company and subcontractor BA of PulseLedger Health Inc. Also 45 freelance coding-QA contractors via a staffing firm |
| What functions? | Coding, claim scrubbing, denials work, engineering support |
| Where is PHI stored? | US only: AWS `us-east-1` (production) and `us-west-2` (DR) under an AWS BAA |
| How is it accessed? | Amazon WorkSpaces VDI in `us-east-1`. No local storage. Clipboard, drive and print redirection **off** (target state; as-is the Chennai policy allowed them, see the worked assessment) |
| Is there a written BAA with the offshore entity? | **Not yet.** The intercompany BAA is missing. Fix it before signing the attestation |
| Staffing firm contractors? | No BAA today. Do not attest until fixed; contractors must not use personal laptops for PHI |
| Audit right and CMS access? | Agree to annual audit and sharing results with the plan and CMS on request |

Never sign an attestation that is true only in the future tense. Say "by [date]" and get the controls in first.

## Common mistakes

- "HIPAA allows it, so we're fine." The customer paper and CMS rules decide.
- Treating a group company as exempt from a BAA because it is "the same company". It is a separate legal entity.
- Forgetting that remote access **is** offshore access, even if the data never leaves `us-east-1`.

## Try this

Draft PulseLedger's answers to the offshore attestation in a one-page table: question, answer, evidence you would attach, and the control that must be live first. Mark every answer that is not yet true. The worked assessment is in [offshore_access_risk_assessment.md](./offshore_access_risk_assessment.md).
