# Using PHI with AI and LLMs (ScribeAssist)

HIPAA has no "AI chapter". The existing rules apply to AI the same way they apply to any system that creates, receives, maintains or transmits PHI. The hard part is that AI projects use data in new ways (training, evaluation, prompt logging) that old BAAs never imagined.

ScribeAssist (launched June 2026) turns a clinician's dictation audio into a transcript, then a third-party LLM drafts a SOAP note and suggests ICD-10/CPT codes. PulseLedger is a business associate (BA) to each provider customer, the covered entity (CE).

## The data flow

```
Clinician dictates ──audio──▶ ScribeAssist (pl-prod-scribe, us-east-1)
                                  │
                                  ├──audio──▶ Speech-to-text vendor   [NO BAA yet]  ◀── fix first
                                  │             (transcript back)
                                  │
                                  ├──prompt (transcript + minimal context)──▶ LLM provider
                                  │                                          (BAA + zero retention)
                                  │◀──────────── draft SOAP note + ICD-10/CPT suggestions
                                  │
                                  ▼
                       Clinician reviews, edits, signs  ──▶ CE's EHR / LedgerRCM coding queue

Side paths to control:  prompt/response logs · eval datasets · human review · dev/test copies (pl-dev)
```

Every box that touches audio, transcripts or notes holds PHI. So does every arrow.

## Permitted purposes: the TPO anchor

A BA may use or disclose PHI only as its BAA allows and only as the CE could (§164.502(a)(3), §164.504(e)(2)(i)). ScribeAssist's core job, helping a provider document care and code claims, falls under **treatment and health care operations** for the provider, and **payment** for coding ([uses_and_disclosures_tpo.md](../03_Privacy_Rule/uses_and_disclosures_tpo.md)). That is fine.

Not fine without more:

- Using customers' PHI to **improve PulseLedger's own products** (training a model that all customers share).
- Selling or licensing outputs.
- Using the PHI for marketing.

## The vendor chain

| Vendor | What it sees | BAA needed? | As-is | Action |
| --- | --- | --- | --- | --- |
| Speech-to-text vendor | Raw audio (voices, names, conditions) | **Yes** | **No BAA** (pilot contract) | Sign a BAA or stop sending production audio. Block at the network level until signed |
| LLM provider | Transcript + prompt | Yes | BAA + zero data retention | Confirm the BAA lists the exact API product and region. Zero retention should be in the contract, not only in a sales email |
| AWS | Everything stored | Yes | BAA signed | Use only HIPAA-eligible services for ScribeAssist data ([safeguards_mapped_to_aws.md](../06_Security_Rule/safeguards_mapped_to_aws.md)) |
| LogPeak (log SaaS) | App logs with names and MRNs | Yes | **No BAA** | Do not send prompts or transcripts to it; scrub logs |

Vendor review detail: [vendor_and_cloud_provider_management.md](../08_Business_Associate_Agreements_and_Vendors/vendor_and_cloud_provider_management.md).

## Training, tuning and evaluation: the real question

**"Can ScribeAssist fine-tune on customer PHI?"** Answer: *Not by default. Only under conditions.*

1. **Check the BAA.** Most BAAs allow use "to provide the services". "To improve the services" or "to develop new products" is a different purpose. If a BAA is silent, assume **no**. About 30 customers use their own paper; each must be read.
2. **Data aggregation** (§164.504(e)(2)(i)(B)) lets a BA combine PHI from several CEs *to support the CEs' health care operations*, if the BAA allows it. It is not a general licence to train PulseLedger's models.
3. **De-identification** by the BA is a use of PHI, so the BAA must permit it. If the BAA allows, de-identify per §164.514(b) (Safe Harbor or Expert Determination) *before* training. Free-text clinical notes and **audio** are hard: names, dates, voices and rare conditions leak. Treat voice as identifying unless an expert says otherwise. See [de_identification_and_limited_data_sets.md](../02_PHI_and_Data_Mapping/de_identification_and_limited_data_sets.md).
4. **Customer opt-in.** For a customer that wants a tuned model for itself only, a signed addendum may allow tuning on its own data. Keep the model and its data siloed per customer.
5. **Individual authorization** is the other route (§164.508) and is rarely practical at this scale.
6. **Model outputs and weights.** A model trained on identifiable PHI can regurgitate it. Treat the weights as PHI unless you can show otherwise. A tuned model that holds one customer's PHI must follow that customer's retention and deletion terms.

Honest summary: with 30 different BAA papers, the default for PulseLedger is **no training on customer PHI**. Evaluation and tuning use synthetic or properly de-identified data, plus opt-in customer addenda.

## Other design points

- **Minimum necessary in prompts** ([minimum_necessary.md](../03_Privacy_Rule/minimum_necessary.md)): send the transcript and what the coding step needs. Do not send MRN, address, SSN or insurance ID unless the task needs them. Replace with tokens; map back after the response.
- **Prompt and response logs are PHI.** Keep them in the same account, same encryption and same retention as the note itself, or turn them off. No ScribeAssist prompts in third-party log SaaS without a BAA.
- **Human review.** The clinician must review and sign. Record who approved and when. AI coding suggestions are *suggestions*; a certified coder verifies before a claim is billed.
- **Hallucination and integrity.** The Security Rule's integrity standard (§164.312(c)(1)) matters here: the note must not silently change after sign-off. Keep version history.
- **Eval datasets.** Same rules as training data. Create a held-out set from de-identified or synthetic data. Access via a named group, logged.
- **Dev copies.** INC-2026-024 (a public bucket in `pl-dev` with copies of production dictation audio, 1,850 patients) is the cautionary tale. No production audio in dev.
- **Retention of audio.** Keep short; see [retention_requirements.md](../10_Retention_Disposal_and_Media/retention_requirements.md).
- **Risk analysis.** ScribeAssist was never in scope of the 2021 "risk analysis". Add it ([risk_analysis_with_worked_example.md](../06_Security_Rule/risk_analysis_with_worked_example.md)).

## Design-review checklist (use before each launch)

| # | Check | Pass when |
| --- | --- | --- |
| 1 | Data inventory | Every store and flow of audio, transcript, prompt, output is listed with an owner |
| 2 | BAA chain | All vendors that touch PHI have a signed BAA that names the service and AI use |
| 3 | Purpose | Each use maps to TPO and to the customer's BAA |
| 4 | Training/eval | No customer PHI is used to train or tune without a written permission and a de-identification record |
| 5 | Minimum necessary | Prompt contents reviewed; identifiers tokenised |
| 6 | Retention | Audio, logs, outputs have TTLs and deletion evidence |
| 7 | Access | Role-based; engineers cannot read production notes by default; break-glass logged |
| 8 | Human in the loop | Clinician sign-off recorded; coder verifies codes |
| 9 | Incident path | Leaked prompt = breach assessment; owner named |
| 10 | Documentation | Decision record, risk register entry, customer-facing description |

## Try this

Write the **AI section of PulseLedger's BAA template** (8 to 12 numbered lines). Cover: permitted AI uses, the ban on training on PHI unless written permission exists, subcontractor AI vendors and zero-retention terms, de-identification rights (if any), logs and outputs as PHI, human review, breach notice for AI incidents, deletion at termination (including tuned models). Mark each line as "required by HIPAA" or "negotiated term".
