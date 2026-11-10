# Health apps and health data outside HIPAA (FTC, Washington MHMDA, states)

Most health apps are not under HIPAA. People assume "health data = HIPAA", and companies assume "not HIPAA = no rules". Both are wrong. This file covers the rules that apply instead, using PulseLedger's consumer app, **PulseWell**, as the example.

## When is an app under HIPAA?

HIPAA reaches a company only if it is a covered entity (CE) or a business associate (BA) ([who_hipaa_applies_to.md](../01_Scope_and_Roles/who_hipaa_applies_to.md)). The question for an app is: **who asked for it, and who is it for?**

OCR's health-app scenarios (2016 "Health App Use Scenarios and HIPAA", check current status) work like this:

| Scenario | HIPAA? |
| --- | --- |
| Consumer downloads a symptom tracker on their own and types data in | **No.** The app developer is not a CE or BA |
| Consumer uses an app and the developer shares data to the consumer's doctor at the consumer's direction | Generally **no** for the developer |
| A provider contracts with the developer for patient-monitoring and the app sends data to the provider's records | **Yes.** The developer is a BA |
| A health plan offers the app to its members and the developer handles plan data | **Yes.** BA |

**PulseWell** (launched 2025) is a direct-to-consumer symptom and medication tracker. Consumers sign up themselves; no provider has engaged PulseLedger for it. So **PulseWell is not HIPAA**. Be careful about two traps:

- If a provider customer ever asks PulseLedger to offer PulseWell *to its patients* and receive the data, PulseWell moves under a BAA for that deployment.
- Do not combine PulseWell data with LedgerRCM or PulseConnect data. A BA may use PHI only as the BAA allows. Mixing would drag PulseWell users' data into the wrong legal bucket and put PHI to a non-permitted use ([ai_with_phi.md](../12_AI_and_Health_Data/ai_with_phi.md) discusses the same issue for models).

## What applies instead

### 1. FTC Act section 5

The FTC can act against **unfair or deceptive** practices. Selling or sharing health data against what your privacy policy says is deceptive. Sharing sensitive health data without clear consent may be unfair. No HIPAA needed.

### 2. FTC Health Breach Notification Rule (HBNR), 16 CFR Part 318

The HBNR covers vendors of personal health records and related entities **not** covered by HIPAA. The FTC finalized amendments on 26 April 2024, published them on 30 May 2024 (89 FR 47028), effective **29 July 2024**. Changes in plain words:

- Health apps and similar tools are clearly covered.
- A "breach of security" now includes **unauthorized disclosure**, not only hacking. Sharing health data with an advertiser without the user's authorization can be a breach.
- Notice goes to affected people, to the FTC, and to the media where 500 or more residents of a state are affected. The clock for individuals is "without unreasonable delay and no later than 60 calendar days" after discovery. For breaches of 500 or more, the FTC notice is due at the same time as individual notice; fewer than 500, an annual report. Check the rule text (16 CFR 318.4 and 318.5) for exact wording: **check current status**.
- Civil penalties apply per violation (the amount adjusts each year; one 2024 source cited up to US$50,120; **check current status**).

### 3. Real FTC cases (verified in outline; amounts you should confirm)

| Case | Year | Outcome | Lesson |
| --- | --- | --- | --- |
| **GoodRx** | 2023 | **US$1.5 million** civil penalty; ban on sharing health data for advertising. First HBNR action | Shared prescription and health data with ad platforms |
| **BetterHelp** | 2023 | Settlement under FTC Act s.5; the FTC announced refunds to consumers (commonly reported as US$7.8 million; **confirm on ftc.gov**) | Shared sensitive questionnaire data with advertisers despite promises |
| **Flo Health** | 2021 | Order requiring user consent before sharing health data | Period-tracking data shared with analytics and ad services |
| **Premom (Easy Healthcare)** | 2023 | Second HBNR action; combined FTC-and-states payments reported up to US$950,000 (**confirm split**) | Fertility-app data to third parties, including overseas analytics firms |

The pattern is the same each time: promise privacy, ship an SDK that sends health data to an ad company.

### 4. Washington My Health My Data Act (MHMDA)

- Enacted April 2023, in force from **31 March 2024** for regulated entities (smaller businesses a few months later). Check dates: **check current status**.
- Covers **consumer health data** that is not HIPAA data. The definition is broad: it includes inferences, location near health facilities, and biometric data.
- Needs a separate **consumer health data privacy policy** (a stand-alone link on the home page), **consent to collect** and a **separate consent to share**, a valid **authorization to sell**, access and deletion rights, and limits on geofences near health facilities.
- **Private right of action** through Washington's Consumer Protection Act. The first lawsuit, *Maxwell v. Amazon.com* (W.D. Wash., filed 10 February 2025), concerns SDK data; its outcome is **check current status**.
- It applies if you target Washington consumers or process Washington residents' data, wherever you sit. PulseLedger Health Inc, in Nashville, still counts.

### 5. Other states (brief)

- **Nevada** (SB 370) and **Connecticut** (consumer health data amendments to its privacy law) adopted consumer-health-data rules. Details and dates vary: **check current status**.
- Many comprehensive state privacy laws treat health data as "sensitive data" needing opt-in consent. See [state_health_privacy_laws.md](../15_Related_Laws/state_health_privacy_laws.md).
- California has its own medical privacy law (CMIA), which can reach app makers.

## PulseWell: obligations map

| Obligation | Source | What PulseLedger must do |
| --- | --- | --- |
| Accurate privacy promises | FTC Act s.5 | Policy matches the code. No health SDKs that send data to ad firms |
| Breach/unauthorized-disclosure notice | HBNR | Incident plan with a PulseWell branch: 60 days, FTC notice, media if 500+ in a state |
| Consumer health data policy and consent | WA MHMDA, NV, CT | Separate policy, collection consent, sharing consent, deletion path |
| Security | FTC expectations | Encryption, access control, vendor review; not the Security Rule, but use the same discipline |
| No mixing with PHI | Contracts and HIPAA | Separate AWS account, separate data stores, no joins with RCM data |
| India | DPDP, CERT-In | If run from Pune, DPDP and CERT-In apply to the Indian operations. See [hipaa_vs_gdpr_vs_dpdp.md](../15_Related_Laws/hipaa_vs_gdpr_vs_dpdp.md) |

### Consent screen sketch

```
+-----------------------------------------------+
|  PulseWell wants to collect your health data  |
|                                               |
|  We will collect: symptoms, medicines, mood,  |
|  notes you enter.                             |
|  Why: to show your trends and reminders.      |
|                                               |
|  [ ] Share with third parties?  (OFF default) |
|      We do not sell or share your data for    |
|      advertising.                             |
|  [ ] Send anonymous trends to improve app     |
|      (separate, optional, OFF default)        |
|                                               |
|  Privacy policy | Consumer health data policy |
|  [ Agree and continue ]   [ Not now ]         |
+-----------------------------------------------+
```

Separate boxes, off by default, no dark patterns, withdrawal in settings.

## Try this

Write the **outline of PulseWell's consumer-health-data privacy policy**: headings only, each with one line of content. Cover: categories collected, purposes, sources, who it is shared with (state the categories of third parties, even if none), how to withdraw consent, how to ask for deletion, contact, and effective date. Then list three SDKs or analytics tools you would refuse to ship in PulseWell.
