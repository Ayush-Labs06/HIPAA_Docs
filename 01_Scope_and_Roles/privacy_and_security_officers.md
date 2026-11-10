# The Privacy Official, the Security Official, and how HIPAA teams really work

HIPAA requires two named people. It does not say how to build a team around them. This file covers what the rules require, what a business associate (BA) actually needs, and who does what at PulseLedger.

## What the rules require

| Role | Rule | Text in plain words |
| --- | --- | --- |
| **Privacy Official** | 45 CFR 164.530(a)(1)(i) | A covered entity (CE) must designate a person responsible for developing and implementing its privacy policies |
| **Contact person** | §164.530(a)(1)(ii) | Someone designated to receive complaints and give information on the privacy notice |
| **Security Official** | §164.308(a)(2) | Identify the person responsible for developing and implementing security policies and procedures |

The designation must be **documented** (§164.530(j), §164.316(b)). A name in an org chart is not enough; keep a dated appointment letter.

**One person or two?** Both rules allow one person to hold both roles, and small practices do. The tension is real: privacy asks "should we use this data?", security asks "is it protected?" Larger firms split them so each has a voice. Combining them is fine if the person has the skills and the time. Putting security under a CIO who is also judged on delivery speed is the usual weak spot.

## Do BAs need both?

- The Security Rule applies to BAs directly, so a BA **needs a Security Official**.
- The Privacy Rule's §164.530 administrative duties are CE duties. A BA is not required by regulation to name a Privacy Official, but its BAAs, customers and sanity all point to naming one. PulseLedger does.
- The BA's officers **support** the CE's officers. At Riverbend, Dana Alvarez is accountable for the hospital's privacy program. PulseLedger's Dr. Leah Morgan makes sure PulseLedger can meet what Dana needs: notice of incidents, support for access requests, a list of subcontractors.

## Who is the workforce?

§160.103 defines workforce as employees, volunteers, trainees and **other persons whose conduct, in the performance of work for the entity, is under its direct control**, whether or not paid. Training (§164.530(b), §164.308(a)(5)), sanctions (§164.530(e), §164.308(a)(1)(ii)(C)) and access controls apply to all of them.

Decide the status of each group and write it down:

| Group | Likely status | Reason |
| --- | --- | --- |
| 560 employees in India | Workforce of the Pvt Ltd | Employed and supervised |
| 45 coding-QA contractors | Workforce **if** PulseLedger directs and controls their work; otherwise the staffing firm is a subcontractor BA | Control test. Either way they need training and records, and the staffing firm needs a BAA if it manages them |
| LogPeak engineers | Not workforce; vendor staff of a BA | No control by PulseLedger |

## Where compliance sits

```
Board / Audit & Risk committee
   │  quarterly report: incidents, risk register, BAAs, audits
CEO
 ├── Chief Compliance & Privacy Officer (Dr. Leah Morgan)  ← Privacy Official
 │      └── HIPAA Compliance Analyst (Priya Nair)
 ├── General Counsel (Marcus Webb)                         ← contracts, breach liability
 ├── CISO (Rohan Kulkarni)                                  ← Security Official
 └── VP Engineering, Head of RCM Ops                        ← control owners
```

- **Compliance** owns policy, training, BAAs, the risk register and incident decisions.
- **Security** owns technical controls, monitoring and the risk analysis method.
- **Legal** owns contract language, privilege and breach-law advice. Involve legal early, but keep legal advice separate from facts, because forensic reports may become discoverable.
- **Operations and engineering** own the controls day to day.

Compliance that reports only to Legal tends to become a contract desk. Security that reports only to Engineering tends to be overruled by deadlines. Give both a line to the CEO or board.

## Board and leadership reporting

OCR looks for evidence that leadership knew. A quarterly one-page report with these items is enough to start:

- Open high risks, with owners and due dates.
- Incidents opened, closed and notified, with timing against clocks.
- BAA coverage: vendors with PHI vs vendors with a signed BAA.
- Training completion, including contractors.
- Open customer audit findings and questionnaire deadlines.

## RACI for PulseLedger's program

R = does the work, A = accountable (one per row), C = consulted, I = informed.

| Activity | Privacy (Leah) | Security (Rohan) | Analyst (Priya) | Legal (Marcus) | Eng (Sneha) | RCM Ops (Farhan) |
| --- | --- | --- | --- | --- | --- | --- |
| Annual risk analysis | C | A | R | I | C | C |
| BAA review and register | A | C | R | R | I | I |
| Incident triage and risk assessment | A | R | R | C | C | C |
| Breach notice to customers | A | C | R | R | I | I |
| Access reviews | C | A | R | I | R | R |
| Training and sanctions | A | C | R | C | I | R |
| Customer questionnaires | A | R | R | C | C | C |

## Try this

Write a one-page RACI for the **response to INC-2026-024** (the dev S3 bucket holding production dictation audio, public for 31 hours). List at least 10 tasks from detection to closure, such as "close public access", "preserve logs", "decide breach", "notify Riverbend", "root-cause fix", "board report". Give each exactly one **A**. Check: who owns the decision that it is a breach? If your answer is "engineering", fix it.
