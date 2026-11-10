# Ransomware tabletop exercise — "The Chennai file server" (INC-2026-031)

A tabletop is a rehearsal with no keyboards. A facilitator reads timed "injects". The group says what it would do. Then the facilitator shows the expected answer. You find the gaps on paper, not at 2 a.m. with real patients.

This replays **INC-2026-031**. Facts below are the incident record. Items marked *(exercise assumption)* are added to make the drill run.

## Setup

- **Roles:** Incident lead (Rohan Kulkarni, CISO), Privacy Official (Dr. Leah Morgan), counsel (Marcus Webb), ops (Farhan Sheikh), engineering (Sneha Joshi), scribe (Priya Nair).
- **Rules:** answer from what your role knows at that time. Log every decision with a time. The scribe's log becomes the evidence file (burden of proof, §164.414; see [breach_notification_60_days.md](./breach_notification_60_days.md)).
- **Facilitator's facts:** found **2026-08-03 02:40 IST** on the Chennai ops-centre file server. Claims export files for Riverbend, Lakeshore (including Part 2 records) and 11 practices were encrypted. Firewall logs show **4.2 GB** left before encryption. Backups are restorable. **61,400** individuals, **3,900** of them Lakeshore SUD patients, in Ohio, Illinois, Indiana and Michigan.

```
T+0h   02:40 IST    ransom note on Chennai file server
T+2h   04:40        scope, egress spike
T+6h   08:40        CERT-In 6-hour mark
T+24h  08-04 02:40  Riverbend BAA clock (24 h)
T+72h  08-06 02:40  forensics: data exfiltrated; attacker demand
T+10d  08-13        counts final (Lakeshore's 5 days ended 08-08)
T+45d  09-17        letters going out; outer limit is 2026-10-02
```

## Inject 1 (T+0h): the note

**Facts:** a night-shift engineer sees renamed files and a ransom note.
**Ask:** Who do you call? What do you touch?
**Expected:** isolate the host from the network; do **not** power off or wipe it (evidence). Open the incident file and start the log. Discovery is **now**, because a workforce member knows (§164.404(a)(2)). Rohan calls Marcus, who engages outside counsel and a forensic firm. Do not contact the attacker.

## Inject 2 (T+2h): what is on it?

**Facts:** the server holds claims exports for Riverbend, Lakeshore and 11 practices. The firewall shows a large outbound transfer.
**Ask:** Is it a breach? Do we wait for forensics before telling anyone?
**Expected:** OCR's 2016 ransomware fact sheet presumes encrypted ePHI was acquired, so breach is presumed unless the four factors show low probability of compromise ([is_it_a_breach.md](./is_it_a_breach.md)). An egress spike removes any realistic chance of that. Do not wait: clocks run from discovery. Build the affected-customer list from file paths. Flag the **Part 2** data. Test that backups are clean.

## Inject 3 (T+6h): CERT-In

**Facts:** it is 08:40 IST.
**Ask:** Is a US HIPAA incident also an Indian reporting event?
**Expected:** yes. The CERT-In Directions of 28 April 2022 require reporting listed incidents, including ransomware and data breaches, within **6 hours** of noticing. The server is in India, run by PulseLedger Technologies Pvt Ltd. Report with what is known, then update. Keep logs (the Directions say 180 days, in India). See [../09_Offshore_and_India/offshoring_phi_to_india.md](../09_Offshore_and_India/offshoring_phi_to_india.md). DPDP breach duties are not yet in force (about 13 May 2027). Check current status of both.

## Inject 4 (T+24h): customers

**Facts:** Riverbend's BAA says **24 hours**, Lakeshore's **5 days**. Some of the 11 practices say 24 hours, 10 days or "without unreasonable delay".
**Ask:** Who do we tell, in what order? Do we wait for more facts?
**Expected:**
- Tell Riverbend (Dana Alvarez) by **08-04 02:40 IST**: what happened, which data, what is unknown. An early honest notice beats a perfect late one.
- Tell **every** affected customer on the shortest clock, not only where their paper demands it ([baa_clause_by_clause_review.md](../08_Business_Associate_Agreements_and_Vendors/baa_clause_by_clause_review.md)).
- §164.410 gives a BA 60 days at most. If PulseLedger is the CE's agent, the CE's own 60 days started at PulseLedger's discovery.
- Tell Lakeshore that SUD records are involved. Notify the cyber insurer per policy and report to law enforcement. Counsel handles any delay request (§164.412).

## Inject 5 (T+72h): forensics and the demand

**Facts:** the forensic firm confirms **4.2 GB** went to an external host. The attackers threaten to publish unless paid. The restore test passed *(exercise assumption)*. The first-entry route is still open *(exercise assumption: facilitator picks phished credential, exposed service or unpatched host)*.
**Ask:** Do we pay?
**Expected:** backups work, so no payment is needed to recover. Payment buys only an unverifiable promise to delete. OFAC's updated advisory (21 September 2021, superseding October 2020) warns that paying or facilitating ransomware payments can breach US sanctions, discourages payment, and counts prompt reporting to law enforcement and strong prior security as mitigating. A November 2026 search found nothing newer; check OFAC's site for current status. Counsel decides. Exfiltration confirmed means **breach confirmed**: the four-factor memo is now a formal record.

## Inject 6 (T+10d): counts and owners

**Facts:** final count **61,400**, including **3,900** Lakeshore SUD patients.
**Ask:** Who sends letters? Who tells HHS? Media? Does Part 2 change anything?
**Expected:**

| Duty | Owner | Rule | Note |
| --- | --- | --- | --- |
| Individual notices | Each CE (PulseLedger drafts if the BAA delegates) | §164.404 | **2026-10-02** at the latest; aim earlier |
| HHS portal | Each CE, its own count | §164.408(b) | A CE with 500+ affected files at the same time as the letters |
| Media notice | Each CE, per state | §164.406 | More than 500 residents of one state; CEs count by state |
| State laws | CEs, often PulseLedger | OH, IL, IN, MI | AG notice may apply; check current statute for each |
| CERT-In update | PulseLedger Pvt Ltd | 2022 Directions | As forensics land |
| Part 2 | Lakeshore | 42 CFR Part 2 | Breach notice aligned with HIPAA under the 2024 rule; check current status ([../13_Special_Contexts/substance_use_records_part_2.md](../13_Special_Contexts/substance_use_records_part_2.md)) |

Write the 3,900 letters with Lakeshore's clinical staff: SUD data is among the most sensitive, and Part 2 limits its use against the patient. Offer a toll-free line and, where SSNs were present, identity protection.

## Inject 7 (T+45d): root causes

**Ask:** The board asks why it happened and whether it will recur.
**Expected:** map causes to [../06_Security_Rule/risk_analysis_with_worked_example.md](../06_Security_Rule/risk_analysis_with_worked_example.md):

| Risk ID | Lesson |
| --- | --- |
| R-14 (Chennai file server) | Predicted at score **20**. Move exports to S3 behind the VDI; EDR; egress alerts |
| R-03 (logging) | Short log retention and missing data events slowed scoping |
| R-12 (Part 2 mixed in) | Tag and segment so one server's loss does not sweep in SUD data |
| R-05 (VDI redirection) | PHI sat outside the VDI path; close the bypass |
| R-13 (training records) | If entry was a phished credential, role training with records is the control |

Also fix: the **missing intercompany BAA** (US Inc to India Pvt Ltd) and the **2014 BAA template** with its vague notice clock.

## Real enforcement

- **Anthem, US$16 million (OCR, October 2018)**: a major cyberattack with no enterprise-wide risk analysis. Lesson: scope everything, as R-14 shows.
- OCR resolution agreements for ransomware commonly cite a thin risk analysis and weak activity review. Check OCR's resolution agreements page for the current list before citing one.

## Debrief scorecard

Score 0 (missed), 1 (late or partial), 2 (right and on time).

| Area | Question | Score |
| --- | --- | --- |
| Containment | Isolated without destroying evidence? | |
| Discovery | Discovery time recorded correctly? | |
| CERT-In | Reported inside 6 hours? | |
| Customers | Riverbend by T+24h; all others on the shortest clock? | |
| Presumption | Treated as breach without waiting for forensics? | |
| Part 2 | Lakeshore told SUD records are in scope? | |
| Ransom | Counsel and law enforcement engaged; sanctions review first? | |
| Evidence | Log, memo, notices dated and filed? | |
| Root cause | Each cause tied to a risk ID, owner, date? | |

Under 12 of 18 means rerun next quarter.

## Try this

Run it with 3 friends, or play every role yourself with a timer. Write your answers before reading the expected ones, then fill the scorecard honestly. Update the notification matrix from [breach_notification_60_days.md](./breach_notification_60_days.md) and keep both as portfolio pieces.
