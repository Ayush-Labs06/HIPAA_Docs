# Managing vendors and cloud providers that touch PHI

A business associate (BA) is only as safe as its own vendors. Under §164.502(e)(1)(ii) and §164.308(b)(2), a BA must get satisfactory assurances, in a written contract, from every subcontractor that creates, receives, maintains or transmits PHI for it. A missing BAA is a violation on its own, and it leaves every upstream covered entity (CE) exposed. For clause detail see [baa_clause_by_clause_review.md](./baa_clause_by_clause_review.md).

## Step 1: find who touches PHI

Start from the data map ([../02_PHI_and_Data_Mapping/ephi_data_mapping_in_the_cloud.md](../02_PHI_and_Data_Mapping/ephi_data_mapping_in_the_cloud.md)), not from the procurement list. Shadow vendors hide in logs, chat, analytics and support tools. Ask finance for every paid tool and engineering for every SaaS URL that logs call.

## Step 2: tier the vendor

| Tier | Test | Examples | Diligence |
| --- | --- | --- | --- |
| 1 Critical | Holds or processes PHI at scale, or an outage stops service | AWS, LLM provider, clearinghouse | BAA, annual evidence, exit plan |
| 2 High | Sees PHI in limited form or volume | SMS and email gateways, speech-to-text, LogPeak | BAA, questionnaire, scoped access |
| 3 Low | No PHI, but touches systems | Dev tools without data access | Standard security review, no BAA |
| 4 None | No PHI, no access | Office supplies | None |

If the answer to "could PHI reach them?" is "maybe", treat as Tier 2 until the data flow is shown otherwise. Logs with names and MRNs are PHI. "We only store logs" is not an exception.

## Step 3: the intake questionnaire

Ask for specifics and evidence, not yes or no:

- Will you sign a BAA? Which services does it cover? (Check **plan-level** scope: a free or standard plan often has no BAA.)
- Where is PHI stored and processed, and who can access it from outside the US?
- Encryption in transit and at rest; key control; MFA; logging.
- Subcontractors you use for our data, and your flow-down.
- Breach notice time, and a named contact.
- Retention and deletion on request and at exit; can you give a certificate?
- Do you train on or reuse our data? (No, in writing.)
- Latest SOC 2 Type II, pen-test summary, and any HITRUST report.

### What reports prove, and what they do not

| Report | Shows | Does not show |
| --- | --- | --- |
| **SOC 2 Type II** | An auditor tested controls against the Trust Services Criteria over a period | That HIPAA is met; it tests only the controls the vendor chose to include. Read scope, period and exceptions |
| **HITRUST** (e1, i1, r2) | An assessment against HITRUST's control framework at different depths; e1 and i1 are 1-year, r2 is 2-year as HITRUST publishes them (check current status) | HIPAA compliance, Privacy Rule duties or a BAA. HITRUST certifies its own assessment, not HIPAA |
| ISO 27001 | An information-security management system | Anything about PHI specifically |
| Questionnaire | The vendor's claims | Anything unless backed by evidence |

There is no HIPAA certification a vendor can hold. If one claims it, treat that as a warning sign and ask for the real evidence ([../14_Compliance_Program_and_Audit/framework_crosswalk.md](../14_Compliance_Program_and_Audit/framework_crosswalk.md)).

## AWS: the BAA and your duties

- Accept the BAA in **AWS Artifact** for the organization's management account so member accounts (`pl-prod-rcm`, `pl-prod-connect`, `pl-prod-scribe`, `pl-dev`, `pl-security`, `pl-sandbox`) are covered. Save the signed copy.
- The BAA covers only **HIPAA-eligible services**. The current list is at https://aws.amazon.com/compliance/hipaa-eligible-services-reference/. PHI belongs only in listed services.
- AWS's job is "security of the cloud". Yours is "security in the cloud": configuring IAM, encryption, logging, network, backups and public-access blocks ([../06_Security_Rule/safeguards_mapped_to_aws.md](../06_Security_Rule/safeguards_mapped_to_aws.md)). Read the current BAA text for its own expectations, such as encrypting PHI.
- An AWS BAA does not fix a public bucket. INC-2026-024 happened inside a signed BAA.
- PulseLedger gap: nobody ever compared the services in use to the eligible list. Fix: export services used per account, compare, record exceptions in the risk register.

## AI/LLM vendors

ScribeAssist uses an LLM provider (BAA, zero data retention) and a speech-to-text vendor (**no BAA**, pilot contract). Require:

- A BAA covering the exact API or product tier used, not the vendor's other products.
- **Zero or contractually limited retention** for prompts, outputs and audio, and no training on customer PHI.
- Sub-processor list; US processing; incident notice.
- No consumer or "free" tiers for PHI, ever.

Details: [../12_AI_and_Health_Data/ai_with_phi.md](../12_AI_and_Health_Data/ai_with_phi.md).

## Chat, collaboration and log SaaS

- **Chat tool** (INC-2026-027): a support engineer pasted 23 patients' data into a plan with no BAA. Options: move to a tier that signs a BAA, or ban PHI and enforce it with DLP. Policy alone failed.
- **LogPeak**: logs carry names and MRNs. Best fix is to stop sending PHI: scrub identifiers at the source, use tokens. If PHI must stay, get a BAA.
- **Google Workspace**: BAA covers core services only. Check the covered-services list before putting PHI in an add-on.

## Subcontractor flow-down

A BAA with a subcontractor must carry restrictions no less strict than your upstream BAAs. The notice clock must be **shorter** than the shortest upstream clock, or you cannot keep your own promise. Cases:

- **Intercompany**: PulseLedger Health Inc needs a BAA with PulseLedger Technologies Pvt Ltd (separate legal entity).
- **Staffing firm**: 45 coding-QA contractors. Decide by facts whether they are workforce or the firm's staff, and paper accordingly; contractors on personal laptops fail either way.

## Offboarding: return or destroy

At contract end (§164.504(e)(2)(ii)(J)): get the data back or destroyed, with a **certificate of destruction** naming systems and dates. If destruction is infeasible, protections continue and use is limited to the reason it cannot be destroyed. Revoke access, rotate keys and API tokens, remove the vendor from logging and chat, and update the register. See [../10_Retention_Disposal_and_Media/retention_requirements.md](../10_Retention_Disposal_and_Media/retention_requirements.md).

## PulseLedger vendor register (before and after)

| Vendor | PHI seen | Tier | BAA before | Action | BAA after |
| --- | --- | --- | --- | --- | --- |
| AWS | All | 1 | Signed, services never checked | Compare services to eligible list | Signed, verified |
| Clearinghouse | Claims | 1 | Signed | Check notice clock | Signed |
| SMS gateway | Names, phone, appointments | 2 | Signed | Review content | Signed |
| Email vendor | Reminders, statements | 2 | Signed | Review content | Signed |
| LLM provider | Notes | 1 | Signed, zero retention | Keep; verify tier | Signed |
| Speech-to-text | Dictation audio | 2 | **None** | Sign BAA or replace | Signed |
| LogPeak | Names + MRNs in logs | 2 | **None** | Scrub or BAA | Signed or removed |
| Chat tool | Pasted PHI | 2 | **None** | BAA tier or ban PHI | Signed or PHI blocked |
| Google Workspace | Email, docs | 2 | Signed, core only | Confirm scope | Signed |
| Staffing firm | QA review | 2 | **None** | BAA or workforce model | Signed |

## Try this

Tier and remediate the 4 no-BAA vendors: speech-to-text, LogPeak, the chat tool and the staffing firm. For each, write the PHI that reaches it, its tier, the decision (sign, replace or stop sending PHI) with a date, and the interim control. Count how many risk-register rows from [../06_Security_Rule/risk_analysis_with_worked_example.md](../06_Security_Rule/risk_analysis_with_worked_example.md) close when you finish.
