# Who HIPAA applies to (covered entities, §160.103)

HIPAA does not follow health data around the world. It attaches to **types of organizations**. Get the role wrong and every later answer is wrong: the contract, the breach clock, the notice, the audit. This is the most important five minutes in the repo.

## The three covered entity (CE) types

45 CFR 160.103 defines a covered entity as one of three things.

| CE type | Plain words | Examples |
| --- | --- | --- |
| **Health plan** | Pays for or provides health care coverage | Insurers, HMOs, Medicare, Medicaid, Medicare Advantage plans, many group health plans |
| **Health care clearinghouse** | Converts non-standard data to or from standard transactions | A billing service that turns a practice's proprietary file into an X12 837 |
| **Health care provider that conducts standard transactions electronically** | A provider who bills or checks eligibility electronically in the standard formats | Hospitals, physician practices, dentists, pharmacies, behavioural health clinics |

**Why the transactions trigger matters.** A provider is a CE only if it transmits health information electronically in connection with a standard transaction (claims, eligibility, remittance, referral authorization, and so on). A cash-only counselor who never bills electronically is not a CE, even though she holds sensitive data. Most providers are CEs because they bill insurers.

## Special structures

- **Hybrid entity** (§164.103, §164.105(a)). One legal entity does both covered and non-covered things, such as a university with a hospital and a business school. It designates its **health care components**. HIPAA applies to those; the rest is outside.
- **Affiliated covered entity, ACE** (§164.105(b)). Legally separate CEs under common ownership or control can designate themselves as one CE, sharing policies and one NPP.
- **Organized health care arrangement, OHCA** (§160.103). A hospital and its independent medical staff, for example, who deliver care together. They can share PHI for joint operations without each being the other's BA.

## Who is not covered

| Entity | Why not |
| --- | --- |
| Employers, in their role as employers | Employment records are excluded from PHI. Their *group health plan* is a CE; the employer is not. See [employers_and_group_health_plans.md](../13_Special_Contexts/employers_and_group_health_plans.md) |
| Life, disability, auto and workers' comp insurers | Not "health plans" under the definition |
| Most consumer apps, wearables, symptom trackers | Not a plan, clearinghouse or billing provider, and not working for one |
| Schools | Student health records are education records under FERPA, excluded from PHI |
| Law enforcement, most courts, most government agencies | Not CEs |
| Data brokers | Same as apps: no CE or BA relationship |

Not covered by HIPAA does not mean unregulated. The FTC and state laws pick up the slack: [health_apps_and_non_hipaa_data.md](../11_Tracking_Tech_and_Health_Apps/health_apps_and_non_hipaa_data.md).

## HIPAA follows the relationship, not the map

HIPAA has no territorial clause. It applies to a US CE wherever its data goes. A US hospital that sends claims to a coder in Pune is still a CE, and the Pune company is a **business associate** (BA) or subcontractor bound by the BAA and, in OCR's view, the Rules. Practical points (my reading, not guidance):

- The lever on an Indian vendor is the **contract chain**, plus the US customer's own liability.
- A foreign BA with no US presence is hard for OCR to reach directly. The US CE still answers for it.
- Customers therefore ask offshore vendors for more controls, not fewer. See [offshoring_phi_to_india.md](../09_Offshore_and_India/offshoring_phi_to_india.md).

## Decision flowchart

```
Does the entity handle health information?
 │ no ──▶ Not HIPAA (maybe FTC/state law)
 ▼ yes
Is it a health plan?  ──yes──▶ CE
 │ no
Is it a clearinghouse? ──yes──▶ CE
 │ no
Is it a health care provider?
 │ no ──▶ Does it work FOR a CE/BA and touch PHI? ──yes──▶ BA (or subcontractor BA)
 │                                                  └─no──▶ Not HIPAA
 ▼ yes
Does it conduct standard transactions electronically (itself or via a billing service)?
 │ no ──▶ Not a CE (state law may still apply)
 ▼ yes
CE ──▶ Is it a hybrid? Designate health care components.
```

The CMS "Are You a Covered Entity?" decision tool follows the same logic. Find the current page on cms.gov (Administrative Simplification) rather than trusting a copy. Run your answers through it as a sanity check.

## PulseLedger

**Is PulseLedger a covered entity? No.** It is not a health plan. It is not a provider. It does turn data into X12 for submission, but it does so *on behalf of* its provider customers, who are the CEs. Because it works for CEs and touches PHI, it is a **business associate**. (Whether a service meets the "clearinghouse" definition is a real question for companies that convert non-standard data for themselves; here the customers' contracts treat PulseLedger as a BA.) Its fully insured group health plan is the insurer's CE concern, not PulseLedger's; see [employers_and_group_health_plans.md](../13_Special_Contexts/employers_and_group_health_plans.md).

**Is PulseWell inside HIPAA? No.** Consumers download it and sign up themselves. No provider or plan engages PulseLedger for it, so PulseLedger is not acting as anyone's BA there. FTC rules (Health Breach Notification Rule) and state laws such as the Washington My Health My Data Act apply.

**Trap.** If a provider customer ever *offered* PulseWell to its patients and PulseLedger ran it for that provider, the answer would flip: PulseLedger would be a BA for that arrangement. The role follows the contract and the facts.

## Common mistakes

- "We hold health data, so HIPAA applies." Not necessarily.
- "We're not a CE, so HIPAA doesn't apply to us." BAs are directly liable under HITECH: [business_associates_explained.md](./business_associates_explained.md).
- Marketing the product as "HIPAA covered". Say what you are, a BA, not a CE.

## Try this

Run these six through the flowchart. For each: CE, BA, or neither, and why.

1. A 12-clinic dental group that bills insurers electronically.
2. A cloud provider that stores a hospital's imaging files and cannot read them.
3. A university whose medical school bills Medicare and whose business school does not.
4. A meditation app a user downloaded herself, where she logs mood and medications.
5. A cash-only physiotherapist who never submits electronic claims.
6. The Pune subsidiary of PulseLedger, coding claims for Riverbend.

Keep your answers. You will compare them with [business_associates_explained.md](./business_associates_explained.md).
