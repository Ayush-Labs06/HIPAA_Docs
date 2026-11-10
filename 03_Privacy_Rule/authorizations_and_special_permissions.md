# Authorizations, marketing, sale of PHI, psychotherapy notes and research (§164.508, §164.501, §164.512(i))

A **HIPAA authorization** is the patient's signed permission for a use or disclosure that the Privacy Rule would not otherwise allow. It is not the same as "consent to treat" and not the same as GDPR consent. It is narrow, specific and revocable.

## When you need one

You need an authorization (45 CFR 164.508(a)) for:

| Situation | Rule |
| --- | --- |
| **Psychotherapy notes** | Almost always, except a short list (below) |
| **Marketing** | Needed, with limited exceptions; if you get paid by a third party, the form must say so |
| **Sale of PHI** | Needed; the form must say the CE gets remuneration |
| Any use or disclosure not covered by TPO, §164.510 or §164.512 | For example, sending records to a life insurer or a patient's employer |

You do **not** need one for treatment, payment or operations, or for the §164.512 public-interest list ([uses_and_disclosures_tpo.md](./uses_and_disclosures_tpo.md), [public_interest_disclosures.md](./public_interest_disclosures.md)).

## Valid authorization: core elements (§164.508(c)(1))

1. A **specific description** of the information.
2. Who may **disclose** it.
3. Who may **receive** it.
4. The **purpose** ("at the request of the individual" is enough when the patient starts it).
5. An **expiration** date or event ("end of the research study" is allowed).
6. **Signature and date.** A personal representative must describe their authority.

Required statements (§164.508(c)(2)): the right to **revoke** in writing and how; whether **treatment, payment, enrollment or eligibility can be conditioned** on signing; and the risk that the recipient may **redisclose** it and it may no longer be protected by HIPAA.

Also: **plain language** (§164.508(c)(3)) and give the individual a **copy** (§164.508(c)(4)).

## Defective authorizations (§164.508(b)(2))

An authorization is invalid if it is: expired; missing a required element or statement; known to be revoked; filled out incompletely; improperly combined with another document (§164.508(b)(3)); or contains false information. A defective authorization gives no cover: disclosing in reliance on it is an impermissible disclosure.

**Conditioning** (§164.508(b)(4)): a CE may not make treatment or payment depend on signing an authorization, with exceptions for research-related treatment, health plan enrollment, and disclosures to a third party for underwriting.

## Marketing after the Omnibus Rule

Marketing (§164.501) means a communication about a product or service that encourages the recipient to buy or use it. Rules:

- **Authorization required** for marketing, **unless** it is a face-to-face communication, or a promotional gift of nominal value.
- If a **third party pays** the CE or BA for sending it ("financial remuneration"), the authorization must say so.
- **Not marketing** (when no third-party payment, or only cost-based payment for refill reminders): treatment communications, refill reminders, case management, and descriptions of the CE's own health-related products.
- A BA may not use PHI for marketing unless the BAA allows it and the CE's rules are met.

**PulseLedger answer:** can PulseConnect send a sponsored message for a pharma brand? **No, not without authorization.** A message paid for by a drug maker that promotes the drug is marketing with financial remuneration. Each recipient must sign a compliant authorization stating the remuneration, and the BAA with the customer must allow it. In practice, say no and offer the customer a non-PHI marketing channel.

## Sale of PHI (§164.502(a)(5)(ii))

A sale means receiving direct or indirect payment in exchange for PHI. It needs an authorization stating that remuneration is involved. Exceptions include public health, research (cost-based), treatment and payment, sale of the business, disclosures to a BA for work on the CE's behalf, to the patient, and required by law. A data broker deal where PulseLedger sells "anonymised" claims is a sale unless the data is truly de-identified under §164.514 ([de_identification_and_limited_data_sets.md](../02_PHI_and_Data_Mapping/de_identification_and_limited_data_sets.md)).

## Psychotherapy notes

**Psychotherapy notes** (§164.501) are a mental health professional's notes that analyze a counseling session and are **kept separate** from the rest of the record. They do **not** include medication, session times, modalities, test results, diagnosis summary, functional status, treatment plan, symptoms, prognosis or progress.

Authorization needed, except: treatment by the originator; training programs; defending the CE in a legal action by the patient; HHS; oversight of the originator; required by law; coroners; and serious-threat prevention. A general **mental health record** is ordinary PHI covered by TPO. Do not confuse the two. At Lakeshore, SUD records have a further regime ([substance_use_records_part_2.md](../13_Special_Contexts/substance_use_records_part_2.md)).

## Research (§164.512(i)) and fundraising (§164.514(f))

- **Research without authorization** needs a **waiver or alteration** approved by an **IRB or privacy board**, with minimal-risk findings. Other routes: reviews preparatory to research, research on decedents, a limited data set with a DUA, or de-identified data.
- **Fundraising** by a CE may use only limited information (demographics, dates of service, department, treating physician, outcome, insurance status). Every message needs a clear **opt-out**, and the NPP must mention it.

## Sample authorization (template)

> **Authorization to Disclose Health Information**
>
> **Patient:** [Name, date of birth]
> **I authorize:** [Practice name] to disclose the following: [e.g., billing statements for dates of service 1 Jan 2025 to 30 Jun 2026]
> **To:** [Recipient name and address]
> **For the purpose of:** [e.g., at my request]
> **This authorization expires on:** [date or event]
>
> **I understand that:** (1) I may revoke this authorization in writing at any time, by sending a written request to [contact], except to the extent action has already been taken in reliance on it. (2) My treatment, payment, enrollment or eligibility for benefits [will not / will] be conditioned on my signing this. (3) Information disclosed may be redisclosed by the recipient and may no longer be protected by federal privacy law. (4) I am entitled to a copy of this form.
> [If marketing or sale: The practice will receive payment from [third party] for this use/disclosure.]
>
> **Signature:** ______  **Date:** ______  [If signed by representative: relationship/authority]

## Try this

Find **three defects** in this authorization. Name the section that each one violates.

> "I, Maria Gomez, authorize PulseLedger and its partners to release all my medical information to any interested party for as long as needed. Signing is required to receive your appointment reminders. Signed: Maria. Date: (blank)."

Hint: look at the description, the recipient, the expiration, the conditioning statement, the revocation statement, the redisclosure statement and the date.
