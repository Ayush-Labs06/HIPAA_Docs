# Information blocking, interoperability and TEFCA

HIPAA says when you **may** share health data. The information-blocking rules say when you **must not obstruct** sharing. They come from a different law, are enforced by different agencies, and catch different players. A compliance analyst at a health-tech business must know both.

## The source of the rules

- **21st Century Cures Act, section 4004** (42 U.S.C. 300jj-52) prohibits information blocking.
- **45 CFR Part 171** (from ASTP/ONC, the HHS health IT office) defines it and lists the exceptions.
- **Information blocking** is a practice that is likely to interfere with access, exchange or use of **electronic health information (EHI)**, where the actor knows (or, for developers and networks, should know) the practice is likely to interfere.
- **EHI** means electronic PHI in a designated record set, whether or not a HIPAA covered entity (CE) holds it. Billing records are in scope; psychotherapy notes and data compiled for legal proceedings are not.

## Who is an "actor"

| Actor | Examples | Enforcer |
| --- | --- | --- |
| **Health care providers** | Hospitals, physician practices, clinics (Riverbend, Lakeshore) | OIG refers to CMS for **disincentives** |
| **Health IT developers of certified health IT** | EHR vendors with ONC-certified products | HHS OIG, civil money penalties |
| **Health information networks / exchanges (HIN/HIE)** | Entities that enable exchange of EHI among more than two unaffiliated parties | HHS OIG, civil money penalties |

If you are not one of these, the Part 171 duties do not apply to you, though your customers' duties may flow into your contracts.

## Penalties (verify before quoting)

- **Developers and HIN/HIEs:** up to **US$1 million per violation**, via OIG, under the civil money penalty rule of 2023.
- **Providers:** no dollar fine. A final rule (**89 FR 54662, 1 July 2024, effective 31 July 2024**) sets **disincentives** through existing programs: for example the Medicare Promoting Interoperability and MIPS programs and the Medicare Shared Savings Program. OIG investigates conduct on or after 31 July 2024.
- **Enforcement activity in 2025-2026:** I found secondary reports of rising scrutiny but **no verified primary-source list of enforcement actions**. **Check current status** on the OIG and ASTP/ONC sites. One report mentioned a newer proposal and a 2026 comment period; I could not confirm it.

## The exceptions

An exception means the practice is **not** information blocking if every condition is met. The set has grown:

| Group | Exception | Plain meaning |
| --- | --- | --- |
| Not fulfilling a request | Preventing harm | Reasonable belief that access would likely cause substantial harm |
| | Privacy | Needed to meet legal privacy conditions, such as missing authorization |
| | Security | Necessary to protect the EHI's security |
| | Infeasibility | Cannot do it (disaster, segmentation, uncontrollable events) |
| | Health IT performance | Planned downtime and maintenance |
| | Protecting Care Access | Added by HTI-2 (Dec 2024); linked to lawful reproductive-health care. **Check current status** |
| Fulfilling a request | Manner | Offer another reasonable way if the requested manner is not possible |
| | Fees | Only reasonable, cost-based fees |
| | Licensing | Interoperability elements licensed on reasonable terms |
| TEFCA | **TEFCA Manner** | Limit fulfilment to TEFCA exchange when both parties are on TEFCA. Added by HTI-2, Dec 2024 (45 CFR §171.403) |

Older guidance says "eight exceptions". One secondary source now counts **ten** in Part 171, including Protecting Care Access and the TEFCA Manner exception. I could not verify the eCFR count directly. **Check current status.**

**HTI-5 (a proposal, with a final rule reported in late 2026):** it proposed to remove the TEFCA Manner exception and narrow the Manner exception. Sources say the final rule (effective about 28 December 2026) **did not finalise** those information-blocking changes and that they stay under review. **Check current status.**

## TEFCA and how it fits

TEFCA is the Trusted Exchange Framework and Common Agreement, a national exchange framework set up under the Cures Act, run through a Recognized Coordinating Entity. Participants sign a common agreement and exchange through qualified health information networks. It is **voluntary**. It matters to information blocking because of the TEFCA Manner exception, and because large providers increasingly join.

## HIPAA and information blocking together

| Situation | HIPAA | Information blocking |
| --- | --- | --- |
| Patient asks for records | Must provide (§164.524, 30 days) | A needless delay can also be blocking |
| Release to another provider for treatment | **Permitted** | May be **required** unless an exception applies |
| Patient asks to send records to an app | Right of access covers it | Refusing without an exception can be blocking |
| State law restricts the release | Preemption analysis | **Privacy exception** may apply if conditions are met |

Rule of thumb: **HIPAA never forces a disclosure to a third party except to the individual and HHS; information blocking can.** A "HIPAA says we can't" answer is wrong when HIPAA in fact permits the disclosure.

## Is PulseLedger an actor?

Work through the tests:

1. **Health care provider?** No. PulseLedger is a business associate (BA), not a provider.
2. **Developer of certified health IT?** Only if it has a product certified under the ONC Health IT Certification Program. LedgerRCM is revenue-cycle software; ScribeAssist drafts notes. Neither is stated to be certified. **Likely not**, but check whether any module is certified.
3. **HIN/HIE?** The test is about enabling exchange of EHI among more than two unaffiliated parties. LedgerRCM sends claims to a clearinghouse and payers. That is mostly financial-transaction traffic, but the definition is broad. **Ask counsel; do not answer from this file alone.**

So: probably **not an actor**, but customers (providers) are, and their contracts may require PulseLedger to support timely access. A BA that stalls a patient-access request for Riverbend could cause Riverbend a problem. See [right_of_access.md](../05_Individual_Rights/right_of_access.md).

## Common mistakes

- Using "HIPAA" as a reason to refuse a disclosure HIPAA allows.
- Charging fees above cost for EHI export.
- Relying on an exception without writing down how every condition was met.
- Treating the exceptions as a blanket: a **practice** is judged case by case.

## Try this

Decide whether each refusal below is information blocking, and name the actor type and any exception you would rely on:

1. A hospital refuses to send records to a patient's new clinic because "we haven't received a BAA".
2. A cloud EHR vendor charges US$500 per patient for an electronic export.
3. A clinic delays a patient's portal access to lab results for three weeks to "let the doctor review".
4. A clinic refuses to send data because the requester's API is down, and offers a secure file transfer instead.

Write one line each. Mark any answer that depends on facts you would need to confirm.
