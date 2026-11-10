# The Notice of Privacy Practices (§164.520)

The **Notice of Privacy Practices (NPP)** is the plain-language document that tells patients how a covered entity (CE) uses their PHI, what rights they have, and what the CE must do. Under 45 CFR 164.520, **CEs** must have one. **Business associates (BAs) do not.** That one fact drives everything PulseLedger does here.

## Who must have an NPP

| Entity | NPP duty |
| --- | --- |
| Health care provider with a direct treatment relationship (Riverbend, a physician practice) | Yes, give it to patients |
| Provider with only an indirect treatment relationship (for example, a reference lab that deals with another provider) | Needs a notice but does not have to distribute it to patients |
| Health plan | Yes, to enrollees |
| Clearinghouse | Not required to distribute to individuals if it only gets PHI as a BA |
| **Business associate (PulseLedger)** | **No** |

## Required content (§164.520(b)(1))

1. **Header**, in this exact wording: "THIS NOTICE DESCRIBES HOW MEDICAL INFORMATION ABOUT YOU MAY BE USED AND DISCLOSED AND HOW YOU CAN GET ACCESS TO THIS INFORMATION. PLEASE REVIEW IT CAREFULLY."
2. **Uses and disclosures:** TPO with **at least one example each**; other permitted and required purposes; any stricter state-law limits.
3. **Authorization statements:** psychotherapy notes, marketing and sale of PHI need authorization; other uses need authorization; the right to revoke.
4. **Fundraising:** the CE may contact the patient and the patient can opt out (if the CE fundraises).
5. **Individual rights:** to request restrictions, confidential communications, inspect and copy, amend, an accounting, and a **paper copy** of the notice.
6. **CE duties:** keep PHI private, provide the notice, follow its terms, and **notify of a breach** of unsecured PHI. State the right to change the terms and how a revised notice is given.
7. **Complaints:** to the CE and to HHS, and a statement of **no retaliation**.
8. **Contact** person or office, and the **effective date**.
9. Health plans: if the plan uses PHI for underwriting, say it will **not** use genetic information for that (§164.520(b)(1)(iii)(C)).

## Distribution and acknowledgement

| Entity | When | How |
| --- | --- | --- |
| Provider | No later than the **first service delivery** (first visit, or first electronic service) | Hand over a copy; make a **good-faith effort to get a written acknowledgement**; if refused, document why |
| Provider in an emergency | As soon as reasonably practicable after the emergency | No acknowledgement required |
| Provider | Post **prominently** at the site and keep copies for people to take | Also on the **website** (§164.520(c)(3)(i)) if the CE has one for its services |
| Health plan | At enrollment; within **60 days** of a material revision; reminder at least every **3 years** | Mail, or email if the individual agrees |

A **material change** may not be implemented before the notice is revised (§164.520(b)(3)). Keep each version for **6 years** (§164.520(e)).

## Layered notices

A CE may put a short **summary** on top of the full notice (§164.520(c)(2)(iii)). The summary is a pointer, not a replacement. The full notice must still contain every required element. Short patient-friendly summaries are good practice: most NPPs run 6 to 10 pages and few patients read them.

An organized health care arrangement (OHCA), such as a hospital and its medical staff, may issue a **joint notice**.

## The Part 2 and reproductive health NPP changes

The 2024 Part 2 final rule (42 CFR Part 2) required NPP changes for entities that handle SUD records. The separate 2024 HIPAA Privacy Rule on reproductive health care also changed the NPP requirements.

- **Status, verified by search (not from memory):** *Purl v. HHS* (N.D. Tex., decided **18 June 2025**) vacated the reproductive-health rule nationwide, **except** the NPP changes tied to Part 2. Reports say CEs still had to update their NPPs for Part 2 by **16 February 2026**, and that NPP updates made only for the reproductive-health parts need revisiting.
- **As of November 2026:** whether HHS appealed, and whether OCR issued enforcement guidance after the 16 Feb 2026 date: **check current status**. Look at the OCR notice-of-privacy-practices page before you tell a customer their NPP is current.
- **Customer takeaway:** CEs, not PulseLedger, own the NPP. But PulseLedger should tell customers when guidance changes. See [substance_use_records_part_2.md](../13_Special_Contexts/substance_use_records_part_2.md) for the Part 2 detail.

## PulseLedger angle: how a BA supports customer NPPs

- **PulseConnect** (portal at `pay.pulseconnect.example`) should **link to the customer's own NPP** in the footer and on the sign-in page, not host a PulseLedger one. Make the link configurable per customer.
- Texts and emails sent for a provider can carry a short link to the provider's NPP if the provider asks.
- **Do not publish a "HIPAA Notice of Privacy Practices" for PulseLedger itself.** It would suggest PulseLedger is a CE and could confuse patients about who holds their data. PulseLedger's *website* privacy policy (`pulseledger.example`) covers only site visitors, and PulseWell has its own consumer privacy policy because PulseWell is not HIPAA.
- If a patient asks PulseLedger for "your NPP", the answer is: "PulseLedger works for your provider. Please contact your provider for their notice." Route the request ([right_of_access.md](../05_Individual_Rights/right_of_access.md)).
- Keep a **version log** of each customer's NPP URL and effective date so the portal links stay current. Tracking pixels on pages that show health information are a separate issue: see [tracking_technologies.md](../11_Tracking_Tech_and_Health_Apps/tracking_technologies.md).
- Where customers' BAAs limit PulseLedger's uses, make sure PulseLedger's practices are consistent with the NPP statements the customer makes (for example, "we do not sell your information").

## Audit checklist

| # | Check | Pass? |
| --- | --- | --- |
| 1 | Exact header wording present | |
| 2 | TPO described with one example each | |
| 3 | Other required/permitted purposes listed (public health, law enforcement, etc.) | |
| 4 | Authorization statements (psychotherapy notes, marketing, sale) | |
| 5 | Fundraising opt-out (if applicable) | |
| 6 | All six rights listed, including paper copy | |
| 7 | Breach notification duty stated | |
| 8 | Right to change terms and how users are told | |
| 9 | Complaint process to the CE and HHS; no retaliation | |
| 10 | Contact name/office and effective date | |
| 11 | Posted on the website (and reachable from the home page) | |
| 12 | Part 2 NPP content present, if the CE has SUD records (check current status) | |
| 13 | Language readable (short sentences; available in common languages of patients) | |

## Try this

Pick a real hospital's NPP (all are public; search "[hospital name] notice of privacy practices"). Run it against the 13-row checklist. Score each row Pass, Partial or Fail, and quote the sentence that proves each Pass. Then answer: does the notice mention Part 2? What would you tell the hospital's privacy officer to change first? Keep the audit notes as a portfolio piece, and do **not** name the hospital as a "bad example" publicly unless you can show a real, verified problem.
