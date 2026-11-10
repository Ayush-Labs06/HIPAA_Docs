# Breach notification: who tells whom, and by when (§164.404–§164.410)

Once you conclude a breach of unsecured PHI happened ([is_it_a_breach.md](./is_it_a_breach.md)), the clocks start. The covered entity (CE) owns the duty to notify individuals, HHS and the media. A business associate (BA) like PulseLedger owes the CE a notice first.

## The clocks at a glance

| Who | To whom | When | Rule |
| --- | --- | --- | --- |
| CE | Each affected individual | Without unreasonable delay, **no later than 60 calendar days after discovery** | §164.404(b) |
| CE | HHS, **500 or more** individuals | At the same time as individual notice (so also within 60 days), via the OCR portal | §164.408(b) |
| CE | HHS, **fewer than 500** | Log all year; report **within 60 days after the end of the calendar year** of discovery | §164.408(c) |
| CE | Prominent media in the state or jurisdiction | If **more than 500 residents** of that state or jurisdiction; within 60 days | §164.406 |
| BA | The CE | Without unreasonable delay, **no later than 60 days after discovery** (plus any shorter BAA deadline) | §164.410 |

60 days is an **outer limit**. "Without unreasonable delay" is the real standard. OCR has fined entities for waiting out the whole 60 days when the facts were known earlier.

## What "discovery" means

A breach is **discovered** on the first day it is *known* to the CE or BA, or would have been known by exercising reasonable diligence (§164.404(a)(2), §164.410(a)(2)). Knowledge of **any workforce member or agent** (other than the person who committed the breach) counts. It does not wait for the compliance team to hear.

If a BA is the CE's **agent** (federal common law of agency), the BA's discovery *is* the CE's discovery, and the CE's 60 days started when PulseLedger found out. That is why BAAs set short notice windows, and why PulseLedger treats its own discovery date as the safest clock for Riverbend.

## Timeline

```
Day 0   PulseLedger workforce member knows (discovery)
 │      Contain; preserve logs; start the incident file
 ▼
Day 0 → BAA deadline (Riverbend 24 h; Lakeshore 5 days; others 24 h–10 days)
 │      PulseLedger notifies the CE: facts, individuals, what PHI
 │      (§164.410 outer limit: 60 days)
 ▼
Day ≤60  CE: individual notices mailed (first-class or agreed email)
 │       500+ individuals → HHS portal filing, same time
 │       >500 in one state → media notice, same time
 ▼
After year end (+60 days)  CE: HHS filing for all <500 breaches
 ▼
6 years  Keep all documentation (§164.414(b), §164.530(j))
```

Worked dates: INC-2026-024 was found **2026-06-20**, so the outer limit for individual and HHS notices (1,850 people, so 500+) is **2026-08-19**. INC-2026-019 (412 people, found 2026-05-12): individuals by **2026-07-11**; HHS log by **2027-03-01**.

## What the individual notice must say (§164.404(c))

Plain language, covering: what happened, with the **date of breach** and **date of discovery**; the **types of PHI** involved; **steps individuals should take** to protect themselves; what the CE is doing to investigate, mitigate and prevent recurrence; and **contact procedures** (toll-free number, email, website or postal address).

Delivery: first-class mail to last known address, or email if the person agreed to electronic notice. Urgent cases (imminent misuse) also get a phone call.

> **Subject: Notice about your health information**
>
> Dear [Patient name],
>
> Riverbend Health System is writing to tell you about an incident involving recordings of your clinic visit dictations. On 20 June 2026, a service provider that helps us with documentation found that a storage folder holding audio copies had been open to the public from about 19 June 2026 to 20 June 2026. The folder was closed the same day.
>
> **What information was involved:** audio of clinician dictations, which may include your name, date of birth, and details about your visit. Social Security numbers and payment card numbers were not included.
>
> **What we are doing:** we closed access, reviewed what could be determined, and are strengthening how test data is handled. We have reported this to the U.S. Department of Health and Human Services.
>
> **What you can do:** be careful with unexpected calls or messages that mention your care. We will not ask for your password or payment details by phone or email.
>
> **Contact:** call [toll-free number] (Monday to Friday, [hours]), or write to [Practice name], [address].

Do not downplay it, and do not claim "no evidence of misuse" if you have no logs.

## Substitute notice (§164.404(d)(2))

When contact details are out of date:

- **Fewer than 10** people: alternative written notice, phone or other means.
- **10 or more**: either a **conspicuous web posting for 90 days** on the CE's home page, or notice in major print or broadcast media where the people live, **plus a toll-free number active for at least 90 days**.

## Law-enforcement delay (§164.412)

If law enforcement says notice would impede a criminal investigation: a **written** statement specifying the time means delay for that time. An **oral** statement means document it and delay for **no more than 30 days** unless a written statement follows.

## Burden of proof (§164.414)

You must be able to show that notices were made, or that no breach occurred. That means the risk assessment, dated copies, mailing proof and the portal receipt. Keep them 6 years.

## Other clocks that stack (check each for the incident)

| Regime | Clock | Note |
| --- | --- | --- |
| Customer BAAs | 24 hours to 10 days | Often shorter than HIPAA. See [baa_clause_by_clause_review.md](../08_Business_Associate_Agreements_and_Vendors/baa_clause_by_clause_review.md) |
| State breach laws | Varies; some fixed at 30 or 45 days; AG notice often at 500+ residents | Ohio, Illinois, Indiana, Michigan for INC-2026-031: check current statute for each |
| FTC Health Breach Notification Rule | Up to 60 days; applies to PulseWell, not to HIPAA data | 2024 amendments in effect from 29 July 2024; confirm thresholds in 16 CFR Part 318 |
| SEC Form 8-K Item 1.05 | 4 business days after a *materiality* determination | Public companies only; applies to customers, not private PulseLedger |
| CIRCIA | Proposed 72 h incident, 24 h ransom payment reports | Final rule not yet in force; check current status |
| **CERT-In (India)** | **6 hours** after noticing a listed incident; keep logs 180 days | Directions of 28 April 2022. Applies to Pune and Chennai incidents |
| DPDP Act and Rules 2025 | Breach duties start about 13 May 2027 (some sources say 14 May) | Notify the Board without delay, detailed report in 72 hours; check the Gazette text |
| 42 CFR Part 2 | Aligned with HIPAA breach notification from compliance date 16 February 2026 | Applies to Lakeshore's SUD records |

## The OCR breach portal

HHS posts breaches of 500+ at https://ocrportal.hhs.gov/ocr/breach/breach_report.jsf, nicknamed the "wall of shame". Listings show entity, state, individuals and type, and mark BA involvement. Customers will read it.

## Real enforcement

- **Presence Health, US$475,000 (OCR, 2017)**: first settlement for late notice. About 100 days against the 60-day limit.

## Try this

Build PulseLedger's notification matrix for INC-2026-031 (ransomware at Chennai, found 2026-08-03 02:40 IST, 61,400 individuals, 3,900 of them Lakeshore SUD patients; Ohio, Illinois, Indiana, Michigan). Columns: obligation, who owes it, trigger, deadline (date and time), owner, evidence. You should end up with more than 10 rows. Compare with [ransomware_tabletop_exercise.md](./ransomware_tabletop_exercise.md).
