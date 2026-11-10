# Business associates and subcontractors (§160.103, §164.502(e), §164.504(e))

For a company like PulseLedger, this is the file that matters most. Almost every health-tech vendor is a business associate (BA). Most get at least one thing wrong about it.

## The definition

Under 45 CFR 160.103 a BA is a person or entity that, **on behalf of a covered entity (CE)**, creates, receives, maintains or transmits PHI for a regulated function (claims processing, data analysis, utilization review, billing, practice management, quality assurance, repricing) **or** provides legal, actuarial, accounting, consulting, data aggregation, management, administrative, accreditation or financial services where PHI is involved.

Since the 2013 Omnibus Rule, a **subcontractor** that does the same for a BA is also a BA. The chain has no fixed depth.

Three tests that settle most cases:

1. Does the vendor work *for* a CE or BA (not for itself)?
2. Does it touch PHI, even only to store it?
3. Is the PHI more than incidental to what it does?

Not BAs: members of a CE's own workforce; one provider sharing PHI with another for treatment; a CE disclosing to a health plan for payment between those two parties.

## The PulseLedger chain

```
Riverbend Health System (CE)
   │  BAA (Riverbend paper: 24h notice)
   ▼
PulseLedger Health Inc (BA)                 ◀── BAA signed (customer paper)
   │  [MISSING: intercompany BAA]
   ▼
PulseLedger Technologies Pvt Ltd (subcontractor BA, Pune/Chennai)
   │  [MISSING: BAA with staffing firm]
   ▼
Staffing firm ──▶ 45 freelance coding-QA contractors (workforce or subcontractor?)

Side branches from PulseLedger Inc:
   AWS (BAA signed)   clearinghouse (signed)   SMS/email vendors (signed)
   LLM provider (signed)   Google Workspace (signed, core only)
   Speech-to-text vendor [MISSING]   LogPeak [MISSING]   chat tool plan [MISSING]
```

Each arrow between BA and subcontractor needs a BAA no less strict than the one above it (§164.502(e)(1)(ii), §164.308(b)(2)). A missing link means PulseLedger is in breach of its own BAAs and every upstream CE is exposed.

Contractors: if the 45 coders work under PulseLedger's direct control, they may be **workforce** (§160.103) and need training, access control and sanctions, not a BAA. If the staffing firm manages them, it is a subcontractor BA. Decide by facts and document it. See [privacy_and_security_officers.md](./privacy_and_security_officers.md).

## BAs are directly liable

Before HITECH (2009) a BA was only liable by contract. Since the Omnibus Rule (2013) OCR can enforce against BAs directly. On 24 May 2019 OCR published a fact sheet, *Direct Liability of Business Associates*, compiling the provisions. In summary (check the fact sheet for the exact list) a BA is directly liable for:

| Duty | Source |
| --- | --- |
| Cooperating with HHS investigations and giving HHS access | §160.310 |
| Notifying the CE (or upstream BA) of a breach | §164.410 |
| Uses and disclosures not permitted by the BAA or required by law | §164.502(a)(3) |
| Giving the CE or the individual access to ePHI | §164.502(a)(4)(ii) |
| Providing an accounting of disclosures | §164.528, §164.504(e)(2)(ii)(G) |
| Signing BAAs with subcontractors | §164.502(e)(1)(ii) |
| Complying with the Security Rule, including a risk analysis | Part 164 Subpart C |
| Taking reasonable steps to cure a subcontractor's material breach | §164.504(e)(1)(iii) |
| Limiting uses, disclosures and requests to the minimum necessary | §164.502(b) |
| Disclosing PHI to HHS when required | §164.502(a)(4)(i) |

A BA is **not** directly liable for everything a CE must do. For instance, it need not issue a Notice of Privacy Practices. Those duties run through the BAA only.

## What a BA may and may not do with PHI

A BA may use and disclose PHI **only as the BAA permits or as required by law**. Extras need written permission from the CE.

- **Own management and administration**, and legal duties: allowed if the BAA says so.
- **Data aggregation** (combining PHI from several CEs to analyse for each CE's operations): allowed only if the BAA grants it.
- **De-identification** is itself a use of PHI. A BA may de-identify and keep the result only if the BAA says so. See [de_identification_and_limited_data_sets.md](../02_PHI_and_Data_Mapping/de_identification_and_limited_data_sets.md).
- **Training models, product analytics, benchmarking**: not permitted unless the BAA covers them. A BA cannot sell PHI or use it for its own marketing.

## The conduit exception (narrow)

A pure conduit, such as the postal service, couriers or an ISP, only transports information and has no routine access. It is not a BA. The test is **transient access**, not "we don't look".

## Cloud providers are BAs even without a key

OCR's 2016 *Guidance on HIPAA & Cloud Computing* says a cloud service provider (CSP) that **creates, receives, maintains or transmits** ePHI is a BA, **even if** it stores only encrypted data and holds no decryption key. "We can't see it" does not make a CSP a conduit. AWS signs a BAA and offers a list of HIPAA-eligible services; the customer must use only those for PHI and configure them correctly. See [vendor_and_cloud_provider_management.md](../08_Business_Associate_Agreements_and_Vendors/vendor_and_cloud_provider_management.md).

## Common mistakes

- Treating a signed BAA as proof of safe handling. It is a promise, not a control.
- Using a free or consumer tier of a tool for PHI because "the enterprise plan has a BAA". INC-2026-027 happened because the chat tool's plan had none.
- No BAA with a vendor that only handles "logs". If logs hold names and MRNs, the vendor is a BA. That is LogPeak.
- Assuming a subsidiary is covered by the parent's BAA. Separate legal entities need their own agreement.
- Letting a BAA's breach-notice clock conflict with the contract below it. A 24-hour promise upstream with a 10-day clause downstream is a breach waiting to happen.

Detailed review: [baa_clause_by_clause_review.md](../08_Business_Associate_Agreements_and_Vendors/baa_clause_by_clause_review.md).

## Try this

Draw PulseLedger's BA chain on paper from the diagram above, adding every vendor in the [meet_pulseledger](../00_Start_Here/02_meet_pulseledger.md) vendor table. Mark each link as **signed**, **missing** or **signed but inadequate** (the 2014 template). Count the missing BAAs. Then rank them by risk, using what PHI each vendor sees. The first one to fix is the one that sees the most PHI with the least control.
