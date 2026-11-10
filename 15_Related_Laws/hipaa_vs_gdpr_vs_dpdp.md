# HIPAA vs GDPR vs India's DPDP Act

PulseLedger lives under all three at once. HIPAA reaches it through US customers. The GDPR can reach it if an EU customer or EU patients appear. India's DPDP Act binds the Pune and Chennai entity and its staff. They look similar and are not. This file is a map, not legal advice.

For the full GDPR treatment, see the sister course: `https://github.com/Ayush-Labs06/GDPR_Docs`.

## Status of the Indian law (as of November 2026)

- **DPDP Act 2023** is law. The **DPDP Rules 2025** were notified on **13 November 2025** (some sources say the gazette date was 14 November).
- Rollout is **phased**: the Data Protection Board provisions took effect on notification; **Consent Manager** registration opens about **12 months** after (around mid-November 2026); the main duties for data fiduciaries start about **18 months** after, i.e. **13 May 2027** (one source says 12 May: check the Gazette).
- The s.17 exemption for processing of non-residents' data under a foreign contract: **check current status** against the Act text. See [offshoring_phi_to_india.md](../09_Offshore_and_India/offshoring_phi_to_india.md).

## The big comparison

| Topic | HIPAA (US) | GDPR (EU/EEA) | DPDP Act 2023 + Rules 2025 (India) |
| --- | --- | --- | --- |
| **Scope trigger** | Entity type: covered entity (CE) or business associate (BA) handling PHI | Establishment in the EU, or targeting or monitoring people in the EU (Art 3) | Digital personal data processed in India; also outside India if offering goods or services to people in India |
| **Who is regulated** | CEs (plans, clearinghouses, providers using standard transactions) and BAs | Controllers and processors | Data fiduciaries (like controllers) and data processors; "significant" fiduciaries have extra duties |
| **Data covered** | PHI: health data plus an identifier, held by a CE/BA | Any personal data; health data is a **special category** (Art 9) | Any digital personal data; **no separate health or sensitive tier** in the Act |
| **Legal basis vs permissions** | No "legal basis". Use is allowed if the Privacy Rule permits it: TPO, public-interest list, or authorization | Need one of six lawful bases (Art 6) and, for health data, an Art 9 condition | Consent, or listed "legitimate uses" (e.g. State functions, medical emergency, employment) |
| **Individual rights** | Access, amendment, accounting, restrictions, confidential communications, complaint | Access, rectification, erasure, restriction, portability, objection, no solely automated decisions | Access, correction and erasure, grievance redress, nominate someone; no portability right |
| **Security duty** | Security Rule: risk analysis, safeguards (§164.306-§164.316) | Art 32: "appropriate" measures, risk-based | Reasonable security safeguards; the Rules list minimum measures |
| **Breach clock** | Individuals and CE: ≤**60 days**; BA to CE without unreasonable delay (§164.404-§164.410) | Authority within **72 hours** (Art 33); individuals if high risk | Notify the Board and each affected person; the Rules require a detailed report to the Board within 72 hours (**verify the wording**) |
| **Processors / BAs** | BAA required (§164.504(e)); BAs directly liable since HITECH | Art 28 contract; processors directly liable | A contract between fiduciary and processor; the fiduciary stays accountable |
| **Cross-border** | **No localisation rule.** Limits come from contracts, CMS and state programmes | Transfers need a mechanism (adequacy, SCCs and so on) | Transfers allowed except to countries the Government restricts by notification |
| **Penalties** | CMP tiers (see [ocr_penalties_and_how_enforcement_works.md](../16_Enforcement_and_Case_Law/ocr_penalties_and_how_enforcement_works.md)); criminal penalties; state AGs | Up to €20 million or 4% of worldwide turnover | Schedule to the Act: up to ₹250 crore per breach category for the heaviest (**verify the schedule**) |
| **Private right of action** | **None** under HIPAA | Yes, damages (Art 82) | No right to compensation in the Act; complaint to the Board |
| **Regulator** | HHS OCR (civil), DOJ (criminal), state AGs | National data protection authorities, EDPB | Data Protection Board of India |

## Plain-English differences that bite

1. **HIPAA is about who holds the data; GDPR and DPDP are about the data itself.** A symptom tracker sold direct to consumers (PulseWell) is outside HIPAA but inside GDPR and DPDP if the people are in the EU or India.
2. **Consent means different things.** HIPAA's authorization is a narrow tool for uses outside TPO. In GDPR and DPDP, consent can be the main basis, and must be specific, informed and withdrawable.
3. **Clocks stack.** A breach can start a 72-hour GDPR clock, a CERT-In 6-hour clock, a customer BAA 24-hour clock and the HIPAA 60-day outer limit all at once. See [breach_notification_60_days.md](../07_Breaches_and_Incidents/breach_notification_60_days.md).
4. **De-identification is not interchangeable.** HIPAA Safe Harbor does not make data "anonymous" under GDPR. See [de_identification_and_limited_data_sets.md](../02_PHI_and_Data_Mapping/de_identification_and_limited_data_sets.md).
5. **Processor language is similar but not identical.** A HIPAA BAA does not satisfy Art 28 or the DPDP contract duty by itself.

## PulseLedger with an EU customer

Suppose a Dutch hospital group asks to use ScribeAssist for its own patients. Now:

- PulseLedger Health Inc would likely be a **processor** under the GDPR. HIPAA does not attach to EU patients alone; it still applies to the US customers.
- It needs an Art 28 contract, an EU transfer mechanism, and records of processing. Dictation audio of EU patients is special-category data.
- The India team that sees the data is a **sub-processor**. Another transfer (EU to India) needs its own mechanism.
- The LLM vendor must be a listed sub-processor, with its zero-retention term in the contract.

**Overlap traps:**

- Using the same US BAA template for an EU customer. It lacks Art 28(3) terms and a transfer clause.
- Assuming "we are HIPAA ready" answers a DPIA question.
- Applying the DPDP outsourcing carve-out (if it holds) to PulseLedger's own Indian employees, who are covered. See [employers_and_group_health_plans.md](../13_Special_Contexts/employers_and_group_health_plans.md).
- Forgetting that the Indian entity is a processor to PulseLedger Inc for US data, but a fiduciary for its own staff data.

## Worked mapping: patient access request

| | HIPAA | GDPR | DPDP |
| --- | --- | --- | --- |
| Who answers | The CE (Riverbend); PulseLedger supports under the BAA | The controller; the processor assists | The fiduciary; the processor assists |
| Deadline | 30 days + one 30-day extension (§164.524) | One month, extendable by two (Art 12) | As set in the Rules, check the current text |
| Fees | Reasonable, cost-based | Free in most cases | Not a feature of the Act, check |

## Try this

Pick **one PulseLedger process**: "A coder in Chennai views a patient's claim history". Fill a three-column table: HIPAA, GDPR (as if the patient were in the EU), DPDP (as if the patient were in India). For each, write the lawful route or permission, the security rule that applies, the breach clock, the rights the patient has, and who is the accountable party. Then circle the rows where the answers differ, and note which contract (BAA, DPA, DPDP processor contract) must cover it.
