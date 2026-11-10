# What counts as PHI (and what doesn't)

Almost every HIPAA decision starts with one question: is this data PHI? If yes, the rules apply. If no, other laws may apply, but HIPAA does not. Get this wrong and you either over-lock harmless data or under-protect the data regulators care about.

## The three-step test

PHI is not "any health data". It is health data **plus** an identifier **plus** the right holder.

```
1. Is it health information?      past, present or future physical or mental health,
   (§160.103 "health information")  care provided, or payment for care
        │ yes
        ▼
2. Is it individually identifiable? (IIHI) - does it identify the person,
   or give a reasonable basis to identify them?
        │ yes
        ▼
3. Is it created, received, held or sent by a covered entity (CE) or
   business associate (BA) in that role?
        │ yes
        ▼
   PROTECTED HEALTH INFORMATION (PHI)  ->  ePHI if electronic (§160.103)
```

Plain words first: a **covered entity (CE)** is a health plan, clearinghouse or provider that sends standard electronic transactions. A **business associate (BA)** is a company that handles PHI for a CE. See [who_hipaa_applies_to.md](../01_Scope_and_Roles/who_hipaa_applies_to.md) and [business_associates_explained.md](../01_Scope_and_Roles/business_associates_explained.md).

- **IIHI** = individually identifiable health information. It is the health + identifier part, before you ask who holds it.
- **PHI** = IIHI transmitted or maintained in any form (paper, electronic, oral) by a CE or BA, with exceptions below.
- **ePHI** = PHI in electronic form. The Security Rule (45 CFR Part 164 Subpart C) protects ePHI only; the Privacy Rule covers PHI in every form.

The same data can be PHI in one hand and not in another. A patient's blood-pressure log on a clinic's server is PHI. The same log typed into a consumer app the patient downloaded alone is not (see [health_apps_and_non_hipaa_data.md](../11_Tracking_Tech_and_Health_Apps/health_apps_and_non_hipaa_data.md)).

## What is excluded

The PHI definition in §160.103 carves out:

| Excluded | Why |
| --- | --- |
| Education records covered by FERPA, and certain student treatment records (20 U.S.C. 1232g) | FERPA governs them instead |
| **Employment records** held by a CE in its role as employer | Employer data, not health-care data |
| Information about a person **deceased more than 50 years** | Time limit in the definition |
| De-identified data that meets §164.514(a)-(b) | No longer identifies anyone ([de_identification_and_limited_data_sets.md](de_identification_and_limited_data_sets.md)) |

Two traps. First, "employment records" does not cover health information a CE collects **as a care provider** about its own staff who are also patients. Second, an employer's sick notes and vaccine status are not PHI, but a group health plan's enrolment and claims data is (see [employers_and_group_health_plans.md](../13_Special_Contexts/employers_and_group_health_plans.md)).

## The 18 identifiers

The Safe Harbor method lists 18 identifier types (§164.514(b)(2)(i)). Even if you never de-identify, this list is the best working checklist for "could this field identify a person?".

| # | Identifier | Notes |
| --- | --- | --- |
| A | Names | Full or partial |
| B | Geographic units smaller than a state | Street, city, county, precinct, ZIP. First 3 ZIP digits allowed only if that area has more than 20,000 people |
| C | Dates (except year) tied to the person | Birth, admission, discharge, death, service dates. All ages over 89 and related dates |
| D | Telephone numbers | |
| E | Fax numbers | |
| F | Email addresses | |
| G | Social Security numbers | |
| H | Medical record numbers | |
| I | Health plan beneficiary numbers | |
| J | Account numbers | |
| K | Certificate or licence numbers | |
| L | Vehicle identifiers, serial numbers, licence plates | |
| M | Device identifiers and serial numbers | |
| N | Web URLs | |
| O | IP addresses | |
| P | Biometric identifiers | Finger and voice prints |
| Q | Full-face photographs and comparable images | |
| R | Any other unique identifying number, characteristic or code | Except a permitted re-identification code |

Note what is **not** on the list: diagnosis, procedure code, drug name. Those are the "health" half. A row of `ICD-10 F32.1, 2026` with no identifier is not PHI. Add a name or an MRN and it is.

## Examples at PulseLedger Health

PulseLedger is a BA to its provider customers for LedgerRCM, PulseConnect and ScribeAssist (see [02_meet_pulseledger.md](../00_Start_Here/02_meet_pulseledger.md)). Here is how the test plays out.

| Data | PHI? | Reasoning |
| --- | --- | --- |
| A claim file (837) for a Riverbend patient | **Yes** | Health, payment, name, DOB, member ID, held by a BA |
| An 835 remittance / EOB batch | **Yes** | Same. INC-2026-019 was this data emailed to the wrong practice |
| Dictation audio from a clinician | **Yes** | Voice is a biometric identifier, and the content is about a patient |
| Appointment reminder SMS: "Hi Maria, your visit at Lakeshore is Tuesday 3pm" | **Yes** | Name + a link to a provider relationship is health information |
| IP address and page URL on the authenticated bill-pay portal | **Yes (likely)** | An IP is identifier O. Combined with "this person logged into a provider's portal" it relates to payment for care. See [tracking_technologies.md](../11_Tracking_Tech_and_Health_Apps/tracking_technologies.md) |
| IP address on the public marketing site `pulseledger.example` | **Not by itself** | No connection to any patient's care; HIPAA does not reach a visitor reading about the company |
| App logs with patient names and MRNs sent to LogPeak | **Yes** | Names + MRNs inside a BA's systems. LogPeak has no BAA, so this is shadow PHI (see [ephi_data_mapping_in_the_cloud.md](ephi_data_mapping_in_the_cloud.md)) |
| A symptom log in PulseWell | **No (not HIPAA)** | Consumer signs up alone; no CE or BA relationship. FTC and state law apply |
| PulseLedger HR file for a Pune coder's sick leave | **No** | Employment record; Indian law (DPDP) governs |
| Aggregate denial-rate dashboard, no patient rows | **No** | No individual is identifiable, but check small cells (see the next file) |
| Analytics set de-identified by Safe Harbor | **No** | Must meet every Safe Harbor condition |

## Common mistakes

- **"It has no name, so it's fine."** An MRN, a device serial, an IP or an exact date can each identify a person. Eighteen types, not one.
- **"Diagnosis alone is PHI."** No. Identifiability is the test.
- **"Encrypted means not PHI."** No. Encrypted PHI is still PHI. Encryption matters for breach rules (see [is_it_a_breach.md](../07_Breaches_and_Incidents/is_it_a_breach.md)), not for scope.
- **"We are not a CE, so it isn't PHI."** A BA holds PHI too. The data is PHI because of who **it came from** and **why you have it**.
- **Treating the filename as safe.** A filename like `Alvarez_Maria_MRN88231.pdf` is itself PHI.

## Try this

Label each of these 12 fields from a LedgerRCM claim record as **identifier (name which of the 18)**, **health/payment info only**, or **both**, then say whether the whole row is PHI and why.

`patient_name`, `date_of_birth`, `member_id`, `mrn`, `date_of_service`, `cpt_code`, `icd10_primary`, `billed_amount`, `zip5`, `payer_name`, `clearinghouse_batch_id`, `coder_notes_free_text`.

Hints: which fields could be kept in a research extract without risk? What would you do about the free-text field? Write your answer as a 12-row table.
