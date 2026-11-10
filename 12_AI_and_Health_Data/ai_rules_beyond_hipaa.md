# AI rules beyond HIPAA: Section 1557, ONC/ASTP transparency and state laws

HIPAA is only one of several rule sets that touch a health AI product. This file is a **map, not legal analysis**. The area moves fast. Every row says what I could verify as of November 2026 and where you must "check current status". Always confirm with counsel before you tell a customer "we are covered".

## The map

| Layer | Who it targets | Hits PulseLedger? |
| --- | --- | --- |
| HIPAA Privacy/Security/Breach | CEs, BAs | Yes, as BA ([ai_with_phi.md](./ai_with_phi.md)) |
| Section 1557 (ACA nondiscrimination) | Health programmes receiving federal funds | Indirectly, via customers |
| ONC/ASTP certification and transparency | Developers of certified health IT | Probably not |
| FDA device rules | Software that diagnoses or treats | Possibly at the edges |
| FTC Act | Deceptive AI claims | Yes (any claim in marketing) |
| State AI and health-AI laws | Providers, payers, developers | Yes, through customers and some direct duties |

## 1. Section 1557, 45 CFR 92.210 (patient care decision support tools)

- The HHS final rule (May 2024) added §92.210: covered entities must make reasonable efforts to identify patient care decision support tools that use race, colour, national origin, sex, age or disability as inputs, and reasonable efforts to **mitigate discrimination risk**. The compliance date for this paragraph was **1 May 2025** (several law-firm sources agree; one lists 2026, which looks like an error).
- It binds the **covered entity** (a hospital, a health plan, a clinic that gets federal funds), not PulseLedger directly. But customers will push the work to vendors by contract: "tell us which inputs your model uses".
- Status after the 2025 change of administration: commentary in 2025 said the rule might be revised. I found **no confirmed 2026 update**. **Check current status** on the HHS OCR Section 1557 page and the Federal Register.
- PulseLedger angle: ScribeAssist's code suggestions and any coding or claim-risk scores. If a score uses age or sex as inputs, document why.

## 2. ONC/ASTP: HTI-1 and what followed

- The HTI-1 final rule (published January 2024) created the **decision support intervention (DSI)** criterion at 45 CFR 170.315(b)(11). Developers of certified health IT had to give users "source attribute" information about predictive models (training data, validation, bias checks) and run risk-management practices.
- It applies to **developers of certified health IT**, not to every AI vendor. PulseLedger does not sell certified EHR technology, so it is likely outside this. Your provider customers' EHR vendor is inside it.
- **HTI-5** (deregulatory) was proposed in December 2025 and, per law-firm reports, finalized in late October 2026, effective **28 December 2026** (some parts 1 January 2027). Reports say it removes the predictive-DSI "model card" transparency requirements and narrows the DSI criterion. These are secondary sources: **check current status** against the Federal Register text. Information-blocking proposals were reportedly not finalized.
- Net: expect less federal transparency law for predictive AI, while customers may still ask for it by contract.

## 3. FDA: software as a medical device and clinical decision support

- Software that is intended to diagnose, treat, cure or prevent disease can be a medical device. The 21st Century Cures Act (section 3060) excludes some **clinical decision support** from device regulation when it is for clinicians, shows its basis, and does not process medical images or signals, among other tests.
- ScribeAssist's SOAP-note drafting is documentation. **Suggesting diagnoses or treatment** would move it toward device territory. ICD-10/CPT suggestions are billing support, a different line, but keep the marketing precise.
- FDA's CDS guidance has been revised from time to time; I could not verify a 2026 revision. **Check current status** on FDA's digital health pages.

## 4. State AI laws touching healthcare

| State law | What it does | Status note |
| --- | --- | --- |
| **California AB 3030** | Health facilities and clinics using generative AI to produce patient clinical communications must include a disclaimer and instructions for reaching a human. Exempt if a licensed provider reviews the message | In effect since **1 January 2025** (secondary sources) |
| **California AB 489** | AI may not imply it holds a healthcare licence | Reported in effect 1 January 2026 |
| **Texas TRAIGA** (HB 149) | Disclosure to patients when AI is used in diagnosis or treatment, plus broader duties | Reported in effect **1 January 2026**; check details |
| **Colorado AI Act** (SB 24-205) | "High-risk" AI used for consequential decisions, including healthcare; developer and deployer duties | Delayed to **30 June 2026** by a 2025 special session. I found no confirmation of what happened after that. **Check current status** |
| Others (Illinois, Utah and more) | Various limits on AI in therapy, mental health chatbots, utilisation review | **Check current status** by state |

There is also federal pressure on state AI laws (a 2025 executive order creating a litigation task force, per commentary). Expect shifts.

## 5. HHS AI strategy

HHS released an AI strategy on **4 December 2025** ("OneHHS"), covering internal use, governance and workforce, and signalling support for AI in clinical decision support. It is a policy document. It does not create duties for PulseLedger. Use it to understand direction, not as a compliance source.

## 6. Contracts: where the real obligations land

Customers will send AI questionnaires. Typical asks: model inventory, training data sources, human oversight, bias testing, incident notice, no training on their PHI, subprocessor list. Answer from facts ([ocr_investigations_audits_and_questionnaires.md](../14_Compliance_Program_and_Audit/ocr_investigations_audits_and_questionnaires.md)).

Also remember **breach and security rules still apply**: an LLM leak is a breach ([is_it_a_breach.md](../07_Breaches_and_Incidents/is_it_a_breach.md)).

## Reading order for a new AI feature

1. Is it PHI? Then HIPAA (BAA, minimum necessary, security).
2. Does it make or influence a care decision for a federally funded customer? Then Section 1557 risk, via the customer.
3. Is it certified health IT? Then ASTP/ONC rules.
4. Does it diagnose or recommend treatment? Then FDA question.
5. Where do patients live and who deploys it? Then state laws (disclosures, licence claims, high-risk AI).
6. What do we claim in marketing? Then FTC.

## Try this

For ScribeAssist's **ICD-10/CPT coding suggestions**, go through the six reading-order steps and write one line each: touches / does not touch / unclear, with the reason. Mark every "unclear" with who you would ask (counsel, a customer's compliance officer, FDA's digital health team). Then list the three facts about the feature that would change your answer, for example "the model suggests a diagnosis shown to the clinician".
