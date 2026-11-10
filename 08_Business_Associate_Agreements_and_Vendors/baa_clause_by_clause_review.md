# Reviewing a BAA clause by clause (§164.504(e), §164.314(a))

A business associate agreement (BAA) is the contract that lets a covered entity (CE) share PHI with a business associate (BA), and a BA share it with a subcontractor. Its required content is fixed by regulation. Everything else is negotiation. This file shows how to review one, using PulseLedger's position as a BA.

Roles for the exercise: the CE is Riverbend Health System. The BA is PulseLedger Health Inc. The subcontractor is PulseLedger Technologies Pvt Ltd or a vendor.

## Required elements (§164.504(e)(2))

| # | Required term | Good looks like | Red flag | PulseLedger's position |
| --- | --- | --- | --- | --- |
| 1 | Permitted uses and disclosures; no use that the CE could not make itself (§164.504(e)(2)(i)) | Lists services (claims, reminders, notes) and nothing broader | "Any purpose related to the Services" | Match each product to a use. ScribeAssist scope stated separately |
| 2 | No other use or disclosure than the contract or law allows (§164.504(e)(2)(ii)(A)) | Plain prohibition | Silent on model training or analytics | No training on customer PHI without written permission |
| 3 | Safeguards and Security Rule compliance for ePHI (§164.504(e)(2)(ii)(B); §164.314(a)(2)(i)) | Refers to Subpart C, not named products | Lists specific tools or "industry best" without limit | Cite the Security Rule; attach a security summary, not a control list |
| 4 | Report impermissible uses, security incidents and breaches (§164.504(e)(2)(ii)(C); §164.410) | Defined "incident", a clock and a content list | "Immediately" with no definition | See notice timing below |
| 5 | Subcontractors accept the same restrictions (§164.504(e)(2)(ii)(D); §164.502(e)(1)(ii)) | Flow-down in writing | Subcontracting banned entirely | Needs the missing intercompany BAA and a staffing-firm BAA |
| 6 | Access to PHI for individuals (§164.524) | Clock short enough for the CE's 30 days | No process | Forward requests to the CE; see [../05_Individual_Rights/right_of_access.md](../05_Individual_Rights/right_of_access.md) |
| 7 | Amendment (§164.526) | Make PHI available and incorporate changes | Silent | Support through the CE |
| 8 | Accounting of disclosures (§164.528) | Keep a log, supply on request | No retention period | Disclosure log, 6 years |
| 9 | Carry out CE duties only as the CE would (§164.504(e)(2)(ii)(H)) | Applies only if BA performs a CE task | Blanket delegation | Limit to tasks actually done |
| 10 | HHS access to books and records (§164.504(e)(2)(ii)(I)) | HHS only | "Any regulator" with no limit | Accept as written |
| 11 | Return or destroy at termination, or extend protections if infeasible (§164.504(e)(2)(ii)(J)) | Timeline, certificate, infeasibility clause | Destruction in 5 days with no backup carve-out | 30 to 90 days; backups age out on schedule |
| 12 | CE may terminate for material breach (§164.504(e)(2)(iii)) | Cure period first | Termination for any breach, no cure | Accept with a short cure period |

Also allowed (§164.504(e)(4)): the BA's own management and administration, and legal duties. **Data aggregation** needs express permission (§164.504(e)(2)(i)(B)). De-identification is a use of PHI, so it needs a clause too ([../02_PHI_and_Data_Mapping/de_identification_and_limited_data_sets.md](../02_PHI_and_Data_Mapping/de_identification_and_limited_data_sets.md)).

## Commonly negotiated, not required

| Term | Typical ask | PulseLedger response |
| --- | --- | --- |
| **Breach notice timing** | 24 hours, 5 days, 10 days, or "without unreasonable delay". HIPAA's own outer limit is 60 days (§164.410) | Offer one standard: initial notice **within 24 hours of confirming** an incident involving the customer's PHI, details to follow. It beats every clock in the book |
| Indemnity | Uncapped, for all breach costs | Cap at a multiple of annual fees; carve in notification costs; check insurance |
| Cyber insurance | Minimum limits | State actual limits; keep evidence |
| **Offshore restrictions** | No PHI outside the US, or approval first | Disclose Pune and Chennai access through the US-hosted VDI; see [../09_Offshore_and_India/offshoring_phi_to_india.md](../09_Offshore_and_India/offshoring_phi_to_india.md) |
| Audit rights | Onsite, any time | Annual, remote first, on notice; accept SOC 2 or HITRUST reports in lieu |
| De-identification rights | Silent or banned | Ask for the right to de-identify for operations only; never for sale |
| Data aggregation | Silent | Needed only for benchmarking across customers; decide explicitly |
| Infeasibility | CE wants all returned | Keep only what law or backup cycles force, with continuing protections |

## Reconciling 30 customer papers

About 30 customers use their own BAA, with conflicting notice clocks (24 hours, 5 days, 10 days, "without unreasonable delay"), and the 2014 PulseLedger template has no subcontractor flow-down. Do not memorize each paper. Build a **clause register** and operate to the strictest term.

Illustrative rows (the clock values follow the case record, the rest are exercise values):

| Customer | Paper | Notice clock | Offshore | Subcontractor consent | Return/destroy | Audit | Gap |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Riverbend | Own | 24 h | Silent | Written notice | 60 days | Annual | None |
| Lakeshore | Own | 5 days | Silent | Silent | 30 days | On request | Part 2 QSOA terms missing |
| Practice A | PulseLedger 2014 | Vague | Silent | None | Silent | None | No flow-down |
| Practice B | Own | 10 days | Banned | Consent | 90 days | Annual | **Offshore conflict** |

Steps: extract each clause into the row, tag conflicts, escalate any ban on offshore access or subcontractors to Marcus Webb, and re-paper the 2014 template users first. Re-check the register after every incident; the INC-2026-031 clocks ([../07_Breaches_and_Incidents/ransomware_tabletop_exercise.md](../07_Breaches_and_Incidents/ransomware_tabletop_exercise.md)) are the test case.

## Redline example

Original (customer paper):

> Business Associate shall notify Covered Entity within twenty-four (24) hours of **any suspected** Security Incident, including unsuccessful attempts.

Redline (PulseLedger):

> Business Associate shall notify Covered Entity within twenty-four (24) hours after **confirming** a Security Incident that involves Covered Entity's PHI, and without unreasonable delay and no later than sixty (60) days after discovery for a Breach of Unsecured PHI. **Unsuccessful attempts such as pings, port scans and blocked log-in attempts require no notice.** Initial notice may be incomplete and will be supplemented as facts are known.

Why: "suspected" starts a clock on noise. A defined trigger and a carve-out for routine attempts are common, reasonable asks.

## Real enforcement

- **North Memorial Health Care, US$1.55 million (OCR, 2016)**: no BAA with a billing vendor and no enterprise risk analysis.
- **Raleigh Orthopaedic Clinic, US$750,000 (OCR, 2016)**: handed PHI of about 17,300 patients to a vendor without a BAA.
- **Care New England, US$400,000 (OCR, 2016)**: the covered hospital had not updated an old BAA for the Omnibus Rule. Lesson for the 2014 template: outdated paper is its own finding.

## Try this

Open HHS's *Sample Business Associate Agreement Provisions* (published January 2013; https://www.hhs.gov/hipaa/for-professionals/covered-entities/sample-business-associate-agreement-provisions/; check that the page is still current). It is sample language, not required. Check each clause against the 12-row table above, mark what is missing, and write one redline in the style above for the breach-notice clause. Add the result to your clause register.
