# Disclosures without authorization: the §164.512 public-interest list

Section 164.512 lets a covered entity (CE) disclose PHI **without** the patient's permission for 12 listed purposes. These are *permissions*, not duties (except "required by law", which is the law's own duty). Each has conditions. Most mistakes come from using the heading and skipping the conditions. The minimum necessary standard applies to most of these ([minimum_necessary.md](./minimum_necessary.md)).

## The 12 categories

| # | Paragraph | One-line rule | Common pitfall |
| --- | --- | --- | --- |
| 1 | (a) **Required by law** | Disclose what the law compels, only to that extent; other (b)–(l) rules still apply to judicial, abuse and law-enforcement cases | Treating "a lawyer asked" as "required by law" |
| 2 | (b) **Public health** | To public health authorities for disease, injury, vital events; to FDA-regulated persons for product safety; to persons exposed to disease; employers for workplace surveillance (limited) | Sending the whole chart when a case report suffices |
| 3 | (c) **Abuse, neglect, domestic violence** | Child abuse reports to authorities; adult victims only when required by law, agreed to, or authorized by law and necessary | Skipping the "inform the victim" step for adult cases |
| 4 | (d) **Health oversight** | To agencies auditing or investigating the health system (licensing, Medicare fraud) | Applying it to investigations of the *patient* |
| 5 | (e) **Judicial and administrative proceedings** | Court order (only what it expressly authorizes) or subpoena/discovery request with *satisfactory assurances* | Releasing on a bare subpoena signed by a lawyer |
| 6 | (f) **Law enforcement** | Six sub-cases (below) | Phone requests from "an officer" without verification |
| 7 | (g) **Decedents** | To coroners, medical examiners and funeral directors | Forgetting PHI of a decedent stays protected for 50 years |
| 8 | (h) **Cadaveric organ, eye, tissue donation** | To organ procurement organizations | Going beyond what donation coordination needs |
| 9 | (i) **Research** | With IRB/privacy board waiver, preparatory review or decedent research | Using "research" for internal analytics |
| 10 | (j) **Serious threat to health or safety** | To prevent a serious and imminent threat, to someone able to prevent it; also to identify escapees | Disclosing for a vague, non-imminent worry |
| 11 | (k) **Specialized government functions** | Military, national security and intelligence, protective services, correctional institutions | Accepting an unverifiable claim of authority |
| 12 | (l) **Workers' compensation** | As authorized by and necessary for workers' comp laws | Handing over the entire record for a back injury claim |

## Subpoena vs court order (§164.512(e))

| Request type | What you may release |
| --- | --- |
| **Court order** (signed by a judge) | Only the PHI the order expressly authorizes |
| **Subpoena or discovery request not signed by a judge** | Only with **satisfactory assurance** that (i) the patient was given written notice and an opportunity to object, with objections resolved or the time passed, **or** (ii) the parties agreed a **qualified protective order** (or have asked the court for one) |
| **Grand jury subpoena** | Treated under law-enforcement rules (f)(1)(ii) |

A CE can also give its own notice or seek a protective order itself instead of waiting for the requester. Document which route you used.

## Law enforcement: the six sub-cases (§164.512(f))

1. **Required by law**, court orders and warrants, grand jury subpoenas, and administrative requests meeting a three-part test (relevant, specific and limited in scope, and de-identified data could not serve).
2. **Identification and location** of a suspect, fugitive, witness or missing person: only eight listed data elements (name, address, date of birth, SSN, ABO blood type, injury, date and time of treatment, physical description). No DNA or dental records.
3. **Victims of crime**: with the victim's agreement, or if they cannot agree and specific conditions are met.
4. **Death suspected to result from crime.**
5. **Crime on the premises** of the CE.
6. **Reporting a crime in an emergency**, off-premises.

## Reproductive health: what changed, what did not

The 2024 HIPAA Privacy Rule on reproductive health care added a ban on certain disclosures and a signed **attestation** requirement for requests related to reproductive health care. Status: *Purl v. HHS* (N.D. Tex.), decided **18 June 2025**, vacated the rule nationwide, except for the Part 2 notice changes covered in [notice_of_privacy_practices.md](../04_Notice_of_Privacy_Practices/notice_of_privacy_practices.md). Reported effect: the attestation requirement is gone and the older Privacy Rule remains. Appeal status and any later developments as of November 2026: **check current status**. State law may still protect reproductive health data; see [minors_representatives_and_sensitive_care.md](../13_Special_Contexts/minors_representatives_and_sensitive_care.md).

## When a subpoena lands at a business associate

A business associate (BA) holds PHI only on a customer's behalf. Its BAA usually says: disclose only as the BAA permits or **as required by law**, and tell the CE promptly.

```
Subpoena/request arrives at PulseLedger
      |
      v
1. Log it (date, who delivered, issuing authority, return date). Stop. Do not release.
      |
2. General Counsel (Marcus Webb) + Privacy Official (Dr. Leah Morgan) review within 1 business day
      |
3. Is it asking for PHI held for a CE customer?
      yes -> tell the CE contact named in the BAA (e.g., Dana Alvarez, Riverbend)
             unless a court order bars notice
      no  -> not PHI-for-a-CE; treat under normal law (e.g., employee records)
      |
4. Let the CE respond, or object to / seek a protective order, within the time left
      |
5. If the CE cannot or the law compels PulseLedger: release only what the order
   requires, minimum necessary, via secure channel, log it for accounting
```

Every release must be logged for the accounting of disclosures ([amendment_and_accounting_of_disclosures.md](../05_Individual_Rights/amendment_and_accounting_of_disclosures.md)). A BA that answers "yes, sure" to a subpoena for a customer's data can breach its BAA and the Rule.

## Try this

A US Attorney's Office subpoena arrives by courier at PulseLedger's Nashville office. It demands "all records relating to patient J. Doe at Riverbend Health System". It is signed by an Assistant U.S. Attorney, not a judge, and gives 14 days to respond.

Write the response steps. Cover: who receives it, what you check first (is it a grand jury subpoena, an administrative request, or a civil subpoena?), who you notify and by when, what you may and may not release, how you log it, and what changes if the subpoena says "do not notify the customer".
