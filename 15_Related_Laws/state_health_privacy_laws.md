# State health privacy and breach laws that sit on top of HIPAA

HIPAA is a federal floor, not a ceiling. States add health privacy laws, consumer health data laws, and breach notification statutes. If you work for a business associate (BA) with customers in many states, you will meet all three.

## Preemption: when the state law wins

45 CFR §160.203 says HIPAA overrides contrary state law, with exceptions. The one that matters most: a state law that is **more stringent** than HIPAA in protecting privacy survives. "More stringent" means, for example, giving individuals greater access or limiting uses and disclosures more tightly (§160.202). State breach laws mostly sit alongside HIPAA, because you can obey both.

Practical rule: **follow HIPAA, then check each state where your patients live for anything stricter, plus any state breach law.** Do not assume HIPAA compliance satisfies them.

## The main laws, with status

### California: CMIA

The Confidentiality of Medical Information Act (Cal. Civ. Code §56 et seq.) covers providers, plans and, since later amendments, some digital health businesses. It restricts disclosure without authorization and gives patients a **private right of action**. For health-tech apps, check whether a product counts as a "provider of health care" under the Act. Not verified in detail here: **check current status** for amendments after 2022.

### Texas: HB 300

Texas Medical Records Privacy Act (Tex. Health & Safety Code ch. 181), the "HB 300" law. Public sources agree on three points:

- A **broader covered-entity definition** than HIPAA. It can reach anyone who collects, stores or transmits protected health information in Texas for commercial, financial or professional purposes, so it can catch BAs, many vendors and certain website operators.
- A **15-business-day** deadline to provide electronic health record access (HIPAA gives 30 days), where the entity uses an EHR that can fulfil the request.
- A **training** requirement for workforce members with PHI access. Sources conflict on timing (for example within 90 days of hire, plus periodic retraining). **Check current status** in the statute before you write a policy.

Penalty figures vary by source; **check current status**.

### Washington: My Health My Data Act (MHMDA)

Washington's law (RCW 19.373) covers **consumer health data** held by businesses outside HIPAA: consent before collection and sharing, a privacy policy, deletion rights, and a ban on geofencing around health facilities. It exempts PHI under HIPAA. It is enforced through the Washington Consumer Protection Act, so it includes a **private right of action**. Sources give main-provision dates of **31 March 2024**, with small businesses on **30 June 2024**, and note some uncertainty over section dates. Confirm in the session law (2023 Wash. Laws 191). PulseWell, the consumer tracker, is exactly in scope. See [health_apps_and_non_hipaa_data.md](../11_Tracking_Tech_and_Health_Apps/health_apps_and_non_hipaa_data.md).

### New York: Health Information Privacy Act (NYHIPA)

This is a status trap. My findings, as of November 2026:

- The original bill passed the legislature and was **vetoed by Governor Hochul on 19 December 2025** (Veto Memorandum No. 135, per secondary sources).
- A **revised bill (S.9269/A.10357)** passed the Senate on 3 June 2026 and the Assembly on 4 June 2026. Industry groups asked for a veto in July 2026.
- I **could not confirm** whether it was signed or vetoed. **Check current status** on the New York Senate and Assembly bill pages before advising anyone. Do not tell a customer NYHIPA is law until you have confirmed.

New York's SHIELD Act (breach and reasonable-security law) is separate and does apply.

### Other states, briefly

Nevada (SB 370) and Connecticut have consumer health data provisions similar in spirit to Washington's; **check current status** for each, as this area changes yearly.

## State breach laws

Every US state has a breach statute. Differences that matter: who must be told (individuals, the **attorney general**, credit bureaus), the deadline, what counts as personal information (many include medical information), and whether HIPAA compliance gives a safe harbor. Several states say a HIPAA-regulated entity that follows HIPAA notification meets its duty, but may still owe AG notice.

## Eight-state comparison for PulseLedger

Check each statute before use. This table is a screening aid, not an answer. Entries marked (v) are details I did not confirm for this file.

| State | Key health or privacy law | Breach notice features (v) | Why PulseLedger cares |
| --- | --- | --- | --- |
| Ohio | General breach law | Deadline stated in statute; AG notice rules (v) | Riverbend is in Ohio; patients in INC-2026-031 |
| Illinois | Breach law (PIPA); mental health records statute | AG notice above a size threshold (v) | Lakeshore is in Illinois |
| Indiana | General breach law | AG notice (v) | Patients in INC-2026-031 |
| Michigan | General breach law | Different structure from OH/IL/IN (v) | Patients in INC-2026-031 |
| California | CMIA; breach law with AG sample notice | AG notice above a threshold (v) | Strictest health privacy private right of action |
| Texas | HB 300 | AG notice for larger breaches (v) | Broad "covered entity" definition |
| Washington | MHMDA | Breach law with AG notice (v) | PulseWell consumers |
| New York | SHIELD Act; NYHIPA (status unclear) | AG and other state agencies (v) | Likely customers and PulseWell users |

## PulseLedger: INC-2026-031 across OH, IL, IN, MI

Patients affected: **61,400**, in Ohio, Illinois, Indiana and Michigan, including **3,900 Lakeshore SUD patients**. The covered entities (Riverbend, Lakeshore, practices) owe HIPAA notices as the CEs. PulseLedger, as BA, notifies them (§164.410) and supports. On top of that:

- Each state's breach statute may require its own content, deadline and **AG notice**. Whether the CE or the vendor sends it depends on the contract and on state law.
- Illinois has stricter handling for certain mental-health and SUD records; Part 2 also applies to Lakeshore. See [substance_use_records_part_2.md](../13_Special_Contexts/substance_use_records_part_2.md).
- A BA that **owns or licenses** data (rather than only maintains it for someone else) may have its own state duty. Counsel decides.

The clocks and a worked matrix are in [breach_notification_60_days.md](../07_Breaches_and_Incidents/breach_notification_60_days.md).

## Common mistakes

- Treating "HIPAA covers us" as the end of the analysis.
- Using one national notice template for every state.
- Skipping AG notice because the individuals were told.

## Try this

List the **extra notices INC-2026-031 needs beyond HIPAA**. For each of the four states, write: who must be told (individuals, AG, other agency), the deadline (look up the statute and cite section), who sends it (PulseLedger or the CE), and the Part 2 angle for Lakeshore. Mark every cell you could not confirm as "check current status".
