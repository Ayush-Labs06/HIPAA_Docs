# OCR penalties and how enforcement really works

Most HIPAA fines are not trials. They are negotiated settlements after a long paper investigation. To work in this field you need to know the four penalty tiers, who else can enforce, and what the process looks like from the inside. Facts here were checked in November 2026; amounts change every year, so re-check the sources.

## Who can enforce

| Enforcer | Power | Legal basis |
| --- | --- | --- |
| HHS Office for Civil Rights (OCR) | Civil: investigations, resolution agreements, civil money penalties (CMPs) | 45 CFR Part 160 Subparts C-E |
| US Department of Justice | Criminal prosecution of knowing misuse of PHI | 42 U.S.C. 1320d-6 |
| State attorneys general | Civil suits for HIPAA violations affecting state residents | HITECH §13410(e), 42 U.S.C. 1320d-5(d) |
| Patients | **No** private right of action under HIPAA. State-law claims (negligence, privacy torts) may use HIPAA as the standard of care | n/a |

A covered entity (CE) or business associate (BA) can face all of these at once for one incident. A BA like PulseLedger is directly liable under HITECH since the 2013 Omnibus Rule (see [business_associates_explained.md](../01_Scope_and_Roles/business_associates_explained.md)).

## The four CMP tiers

The tier depends on **culpability**, not on harm. Source: 45 CFR 160.404(b), amounts adjusted for inflation in 45 CFR 102.3. The latest adjustment I found was published in the Federal Register on **28 January 2026** (91 FR, doc. 2026-01688) and uses OMB's 1.02598 multiplier. The table headed "2025" in that notice is the current one as of November 2026 (check the notice for the next adjustment).

| Tier | Culpability | Min per violation | Max per violation | Calendar-year cap (identical provision) |
| --- | --- | --- | --- | --- |
| 1 | Did not know, and with reasonable diligence would not have known | **US$145** | US$73,011 | US$2,190,294 |
| 2 | Reasonable cause, not willful neglect | **US$1,461** | US$73,011 | US$2,190,294 |
| 3 | Willful neglect, corrected within 30 days | **US$14,602** | US$73,011 | US$2,190,294 |
| 4 | Willful neglect, not corrected within 30 days | **US$73,011** | US$2,190,294 | US$2,190,294 |

For comparison, the 2024 figures were US$141 / 1,424 / 14,232 / 71,162 minimums and US$2,134,831 as the cap. The 2025 numbers come straight from the Federal Register table, not from vendor blogs, which disagreed with each other.

### The 2019 enforcement-discretion notice

The 2013 Omnibus Rule text reads as one calendar-year cap for all four tiers. On **30 April 2019** (84 FR 18151) HHS announced that, pending rulemaking, it would apply **different annual caps per tier**, in 2019 dollars and "as adjusted for inflation":

| Tier | Annual cap in the notice |
| --- | --- |
| 1 | US$25,000 |
| 2 | US$100,000 |
| 3 | US$250,000 |
| 4 | US$1,500,000 |

I did not find a published inflation-adjusted version of these four numbers. Treat the table above as the legal ceiling and the 2019 notice as OCR's lower working cap: **check current status** before quoting an adjusted figure to a client.

### Factors that move the number

45 CFR 160.408 lists them: nature and extent of the violation, harm, history of compliance, financial condition, and other matters of justice. Since Pub. L. 116-321 (2021), OCR must also consider whether the entity had **recognized security practices** in place for the prior 12 months (see [security_rule_explained.md](../06_Security_Rule/security_rule_explained.md)). It is a mitigating factor, not a defense.

## Criminal and state enforcement

- **Criminal (42 U.S.C. 1320d-6):** knowingly obtaining or disclosing PHI in violation of HIPAA. Up to US$50,000 and 1 year; up to US$100,000 and 5 years if under false pretenses; up to US$250,000 and 10 years if intent to sell, transfer or use for commercial advantage, personal gain or malicious harm. Cases are prosecuted by DOJ, usually against individuals (snooping, identity theft).
- **State AGs (HITECH):** up to US$100 per violation and US$25,000 per year for identical violations, plus attorney's fees. In practice AGs usually add their own state consumer-protection and breach laws. Example in [big_cases_and_lessons.md](big_cases_and_lessons.md): the 2020 multistate Anthem settlement.

## *MD Anderson v. HHS* (5th Cir., 14 January 2021)

OCR imposed a **US$4,348,000** CMP on the University of Texas MD Anderson Cancer Center after the loss and theft of unencrypted devices (more than 35,000 individuals). The Fifth Circuit (No. 19-60226) vacated it. Reasons reported in the opinion and commentary:

- HHS acted arbitrarily and capriciously by treating like cases differently, with no reasoned explanation.
- The Security Rule's encryption standard requires a **mechanism** for encryption, and MD Anderson had one.
- Losing devices is not automatically a "disclosure" unless someone outside actually received the PHI.
- HHS conceded it could not defend a penalty above **US$450,000**.

**Effect:** the opinion binds only the Fifth Circuit (Texas, Louisiana, Mississippi), but entities elsewhere now cite it when contesting a CMP. HHS's 2019 notice predates it, so I cannot say the case caused the notice. Primary source: the opinion PDF linked in [sources_and_guidance.md](../references/sources_and_guidance.md).

## OCR statistics

| Figure | Value | Source and date |
| --- | --- | --- |
| Complaints since April 2003 | **374,321** | OCR Enforcement Highlights, data to 31 October 2024 |
| Resolved | 370,578 (about 99%) | same |
| Cases ending in a settlement or CMP | **152**, totalling **US$144,878,972** | same |
| New complaints in calendar 2024 | 30,256 (as summarized from the 2024 Report to Congress; I did not open the PDF) | HHS 2024 Report to Congress |
| Right of Access Initiative | **55 actions**, the 55th announced 27 August 2026 (Azul Vision) | HHS press release (via search); see below |
| Ransomware | 19 completed investigations; 13 under the Risk Analysis Initiative, after four settlements totalling **US$1,165,000** announced 23 April 2026 | HHS press release (via search) |

Most complaints end with no violation or with technical assistance. A settlement is the exception, not the norm. I found no official cumulative figures after October 2024; a vendor count of 153 actions through March 2026 is unverified, so **check current status** on the OCR pages.

## How a case runs

```
Complaint / breach report / media story / audit
        |
        v
OCR opens a review ---> data request letter
        |
        +--> No violation / technical assistance  (most cases)
        |
        +--> Violation found
                |
                +--> Informal resolution: Resolution Agreement + CAP (2-3 yrs)
                |
                +--> No deal: Notice of Proposed Determination (CMP)
                          |
                          +--> request ALJ hearing --> appeal to DAB --> court
                          +--> waive hearing --> final CMP (Warby Parker, 2024-25)
```

- **Negotiation:** OCR proposes an amount from culpability, harm, size and history; the entity argues financial condition and mitigation. Settlements carry "no admission of liability".
- **The CAP** (corrective action plan) is the real cost: a risk analysis redone, policies rewritten, training, and reports to OCR for typically two years.
- **Timeline:** it can be slow. Montefiore's insider theft was first flagged to it by police in 2015 and the settlement came in February 2024.
- **Initiatives:** OCR announces themes: Right of Access (since 2019) and the Risk Analysis Initiative (first action 31 October 2024). OCR's 2025-26 priorities and any HHS reorganization: **check current status**.

## PulseLedger angle

PulseLedger is a BA. Its as-is risk analysis is a 2021 questionnaire that left out ScribeAssist, PulseWell and the Chennai centre (see [risk_analysis_with_worked_example.md](../06_Security_Rule/risk_analysis_with_worked_example.md)). That is the pattern in nearly every recent OCR security case. Cases that match are in [big_cases_and_lessons.md](big_cases_and_lessons.md).

## Try this

Estimate PulseLedger's exposure for the missing enterprise-wide risk analysis (§164.308(a)(1)(ii)(A)) using the table above. Write down: (1) which tier you would argue and why (hint: Tier 1 is hard to defend after 2021, so is it Tier 2 or 3?), (2) the statutory maximum for one identical provision in one calendar year, (3) the lower 2019 working cap for that tier, (4) three facts from [meet_pulseledger.md](../00_Start_Here/02_meet_pulseledger.md) an investigator would use against you, (5) three facts that count as mitigation. Then state what the CAP would require, since that costs more than the fine.
