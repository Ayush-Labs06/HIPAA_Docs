# The right of access, end to end (§164.524)

Patients can ask for a copy of their health records. The covered entity (CE) must give it to them, fast and cheaply. This is the right OCR enforces most often, and the one a business associate (BA) like PulseLedger gets wrong most easily, because the request lands in the wrong inbox.

## The rule in one paragraph

An individual has a right to inspect and get a copy of PHI in a **designated record set (DRS)** for as long as the CE keeps it (§164.524(a)(1)). The CE must act within **30 days**, with at most **one 30-day extension** if it tells the individual in writing why and when (§164.524(b)(2)). It must give the copy in the form and format asked for if it is readily producible, including electronic form for ePHI (§164.524(c)(2)). It may charge only a reasonable, cost-based fee (§164.524(c)(4)).

## What is the designated record set?

Defined in §164.501. In plain words: the records the CE uses to make decisions about the person.

| In the DRS | Usually not in the DRS |
| --- | --- |
| Medical and billing records of a provider | Psychotherapy notes (kept separate) |
| Enrollment, payment, claims adjudication records of a health plan | Quality-improvement and peer-review files |
| Other records used to make decisions about individuals | Raw audit logs, security logs, business-planning files |

For PulseLedger's customers, **claims, remittances (835), coding notes and billing statements are DRS**. They are billing records. So an RCM BA holds DRS data, even though it never treats anyone. Dictation audio and SOAP drafts in ScribeAssist become DRS once the clinician adopts them into the chart; the customer decides.

## Timeline and form

```
Day 0        Request received by CE (written request allowed by CE policy)
Day 0-1      Verify identity (reasonable, not burdensome)
Day 30       Deadline to provide access or a written denial
Day 30       One-time extension notice (reason + new date), if needed
Day 60       Hard stop. No second extension.
```

- **Format:** ePHI in the electronic form requested if readily producible, otherwise an agreed readable form. Unencrypted email is allowed if the individual is warned of the risk and still chooses it.
- **No reasons needed.** The CE cannot ask why, and cannot make the person use a portal or come in person.
- **Third-party direction:** if the individual asks in a signed, written request that the copy go to someone else (a lawyer, an app), the CE must send it to the named recipient (§164.524(c)(3)(ii)).

## Fees

Allowed: labour for copying (paper or electronic), supplies (paper, a USB drive), postage, and a summary only if the individual agrees to it and its fee. **Not allowed:** search and retrieval labour, system costs, or per-page charges that go beyond actual cost.

*Ciox Health, LLC v. Azar*, No. 18-cv-0040 (D.D.C., 23 Jan 2020): the court held that the 2013 Omnibus Rule's third-party directive applies only to **electronic copies of PHI in an EHR**, and that the cost-based fee limit applies only to an individual's request **for their own records**. OCR's notice of 28 Jan 2020 said it would enforce accordingly. Effect: a request made by or for the patient personally has the cap; a request from a lawyer or records-retrieval company is not capped by §164.524(c)(4) (state law fee schedules may still apply). Commentary I found did not confirm an appeal outcome: **check current status** before relying on the details.

## Denials

| Type | Grounds (§164.524(a)) | Review? |
| --- | --- | --- |
| **Unreviewable** | Psychotherapy notes; info compiled for a legal proceeding; certain CLIA lab results; inmate requests that threaten safety; research in progress (if agreed); Privacy Act records; info from a confidential source | No |
| **Reviewable** | A licensed professional decides access is reasonably likely to endanger life or safety; the PHI refers to another person and access would cause substantial harm; a personal representative's access would cause substantial harm | Yes, by a different licensed professional |

The denial must be written, in plain language, give the basis, explain review and complaint rights (to the CE and to HHS), and still give access to any part not denied. "We are worried about the bill" or "the record is incomplete" are not grounds.

## Real enforcement: OCR Right of Access Initiative

OCR launched the Initiative in 2019 and pursues CEs that miss the 30 days or overcharge. **As of November 2026:** HHS's press release on Azul Vision (27 Aug 2026) calls that settlement the **55th** Right of Access enforcement action; a secondary digest reports a 56th in October 2026. I could not confirm 56 on an OCR page: **check current status** on OCR's enforcement page.

| Matter | Year | Amount | Lesson |
| --- | --- | --- | --- |
| Bayfront Health St. Petersburg | 2019 | $85,000 + CAP | First action under the Initiative. A mother waited far beyond 30 days for records about her unborn child |
| Dr. Robert Glaser (New Hyde Park, NY) | 2020 | $100,000 civil money penalty | Request from 2013 never fulfilled, and he ignored OCR's letters: a CMP for willful neglect, no appeal right |
| Elite Primary Care (Georgia) | 2020 | $36,000 + 2-year monitoring | The 13th action; small practices are not exempt |
| Azul Vision | 2026 | $50,000 + CAP | Failed to give PHI within 30 days; still happening in 2026 |

Pattern: small amounts, but a CAP and public naming. More cases in [../16_Enforcement_and_Case_Law/big_cases_and_lessons.md](../16_Enforcement_and_Case_Law/big_cases_and_lessons.md).

## The BA role

A BA does not own the patient relationship. The BAA must require the BA to make PHI available so the CE can meet §164.524 (§164.504(e)(2)(ii)(E)). OCR's 2019 BA fact sheet also lists direct BA liability for giving an electronic copy to the CE or individual as the BAA directs (verify the wording before quoting). Details: [../01_Scope_and_Roles/business_associates_explained.md](../01_Scope_and_Roles/business_associates_explained.md); clause review: [../08_Business_Associate_Agreements_and_Vendors/baa_clause_by_clause_review.md](../08_Business_Associate_Agreements_and_Vendors/baa_clause_by_clause_review.md).

## PulseLedger workflow

As-is: forwarded requests sit in a shared inbox with no clock. The fix:

```
Patient emails PulseLedger support ("send me all my records")
        │  Day 0: ticket opened, clock starts
        ▼
Triage: which CE(s) hold the data? (LedgerRCM client tag)
        │  Day 0-2: acknowledge to patient; do NOT send PHI
        ▼
Forward to CE privacy officer (e.g. Riverbend: Dana Alvarez)
        │  within 2 business days, or BAA clock if shorter
        ▼
CE decides, verifies identity, approves scope
        │  PulseLedger pulls DRS extract from pl-prod-rcm
        ▼
Secure delivery by CE (or by PulseLedger on CE's written instruction)
        ▼
Close ticket, log request, keep 6 years (§164.530(j))
```

Rule: PulseLedger never decides, denies or fees an access request. It routes and supports. Sending records straight to an unverified emailer is itself a possible impermissible disclosure.

### Request log

| ID | Received | CE | Route date | CE due (30d) | Extension? | Closed |
| --- | --- | --- | --- | --- | --- | --- |
| AR-0007 | 2026-03-02 | Riverbend | 2026-03-03 | 2026-04-01 | No | 2026-03-20 |
| AR-0008 | 2026-03-09 | Lakeshore (Part 2 in scope) | 2026-03-10 | 2026-04-08 | Yes, to 2026-05-08 | 2026-04-30 |

Part 2 content needs its own check: [../13_Special_Contexts/substance_use_records_part_2.md](../13_Special_Contexts/substance_use_records_part_2.md). Information-blocking overlap: [../15_Related_Laws/information_blocking_and_interoperability.md](../15_Related_Laws/information_blocking_and_interoperability.md).

## Try this

A patient emails PulseLedger support: "Send me all my records. I know you have them." Write (1) the acknowledgement reply that does not confirm which CE, if any, holds data, (2) the forward note to the CE, (3) the log row, and (4) the day-30 and day-60 dates. Then say what changes if the sender is a lawyer asking for a copy to be sent to the firm.
