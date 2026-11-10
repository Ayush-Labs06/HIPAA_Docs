# Is it a breach? Unsecured PHI and the four-factor assessment (§164.402)

Not every security incident is a breach, and not every impermissible disclosure needs a letter. But the burden runs one way: you must **prove** it was not.

## The definition

A **breach** (45 CFR 164.402) is the acquisition, access, use or disclosure of **PHI** in a manner not permitted by the Privacy Rule that **compromises the security or privacy** of the PHI.

Two rules shape everything:

1. **Presumption of breach.** An impermissible use or disclosure is *presumed* to be a breach **unless** the covered entity (CE) or business associate (BA) shows a **low probability that the PHI has been compromised**, using a documented risk assessment.
2. **Only unsecured PHI counts.** The notification duties apply to *unsecured* PHI: PHI not rendered unusable, unreadable or indecipherable using the HHS-specified methods.

## Three statutory exceptions (§164.402(1))

Not a breach, if:

| Exception | Example |
| --- | --- |
| (i) Unintentional acquisition, access or use by a workforce member acting in good faith and within their authority, with no further impermissible use | A coder opens the wrong claim for a second, closes it |
| (ii) Inadvertent disclosure between two people authorized to access PHI at the same CE, BA or organized health care arrangement, with no further impermissible use | One PulseLedger coder sends a file to another authorized coder on the same client |
| (iii) Good-faith belief that the recipient could not reasonably have retained the information | A misdelivered envelope returned unopened |

Document which exception you rely on. If none fits, run the four-factor assessment.

## The four factors (§164.402(2))

| # | Factor | Ask |
| --- | --- | --- |
| 1 | **Nature and extent of the PHI**, including identifiers and likelihood of re-identification | SSN, diagnosis, financial detail, SUD data = worse. Names alone = less |
| 2 | **The unauthorized person** who used it or received it | Another HIPAA CE with its own duties = lower. A criminal or the open internet = high |
| 3 | **Whether PHI was actually acquired or viewed** | Opened and read, or only exposed? Logs decide. No logs = you cannot say it was not |
| 4 | **Extent to which the risk has been mitigated** | Written assurance of deletion, forensic confirmation, retrieval of the media |

Weigh them together and write the reasoning. If any factor is unknowable, the default leans to "breach".

## "Secured" PHI: the safe harbor

HHS guidance (April 2009, in 74 FR 19006) lists two ways PHI is "secured":

- **Encryption** consistent with NIST (SP 800-111 for data at rest; NIST-validated TLS/IPsec options for data in motion), where the **decryption key was not breached or kept with the data**.
- **Destruction**: paper shredded or destroyed so it cannot be reconstructed; electronic media cleared, purged or destroyed per NIST SP 800-88.

Practical meaning: a lost, fully encrypted laptop is usually not a notifiable breach. A "private" bucket encrypted with a key that the attacker's role could also use is not secured. Redaction and access controls do not count.

## Ransomware

OCR's 2016 fact sheet *Ransomware and HIPAA* says: when ePHI is encrypted by ransomware, the ePHI was *acquired* (an attacker took control), so a breach is **presumed** unless you demonstrate a low probability of compromise with the four factors. Evidence of exfiltration ends the argument. Even encryption of data that was already encrypted at rest may not help, because the attacker's possession is the issue.

## Worked assessments

### INC-2026-019 (found 2026-05-12): 835 batch to the wrong practice, 412 people

A coder emailed an 835 remittance/EOB batch for Practice A to Practice B. Practice B confirmed deletion in writing on 2026-05-13.

- **Argue "low probability":** factor 2, Practice B is itself a HIPAA CE bound to protect PHI; factor 3, no indication anyone used it; factor 4, written deletion confirmation within a day. Factor 1 is moderate (names, member IDs, amounts, procedure codes).
- **Argue "breach":** it went by ordinary email, possibly unencrypted, so other copies may exist (mail server, backups, mobile devices); a written *promise* is not forensic proof; 412 people is a lot; exception (ii) does not apply because B is a different entity.
- **Reasonable outcome:** most teams conclude low probability **with documentation**: B's signed attestation, mail-server purge confirmation, and a note that the CE (Practice A) makes the final call. PulseLedger as BA reports the facts to A promptly under the BAA; A decides. If A concludes breach, 412 is under 500, so it goes in the HHS annual log (see [breach_notification_60_days.md](./breach_notification_60_days.md)).

### INC-2026-024 (found 2026-06-20): public bucket for 31 hours, 1,850 people

`pl-scribe-audio-dev` held production dictation audio of patients of three Riverbend clinics. S3 server access logs were **off**.

- Factor 3 fails: PulseLedger cannot show that nobody downloaded the audio. The internet was the recipient (factor 2), the content is voice with clinical detail (factor 1), and mitigation (closing the bucket) came late (factor 4).
- **Conclusion: breach.** The inability to prove "not accessed" is itself the finding. This is why audit logging is a safeguard, not paperwork ([safeguards_mapped_to_aws.md](../06_Security_Rule/safeguards_mapped_to_aws.md)).

### INC-2026-027 (found 2026-07-08): PHI pasted into a chat tool, 23 people

A support engineer pasted names, DOB, member IDs and CPT codes into the company chat tool, whose plan has **no BAA**.

- A disclosure to a vendor that handles PHI without a BAA is an impermissible disclosure (§164.502(e)). Exception (ii) fails: the vendor is not a person authorized under a BAA.
- Factors: 1 is moderate; 2 is a vendor with contractual confidentiality but no HIPAA duties; 3 depends on whether vendor staff can read workspace content; 4 turns on deletion, retention settings and whether the vendor will sign a BAA retroactively.
- Outcome depends on facts, but with 23 people and a limited audience it is often assessed and documented; document either way.

## Real enforcement

- **North Memorial Health Care, US$1.55 million (OCR, March 2016)**: disclosed PHI to a billing vendor without a BAA; also no enterprise risk analysis. Lesson: "a vendor handled it" is its own violation.
- **Presence Health, US$475,000 (OCR, January 2017)**: first OCR action for **late** breach notification (about 100 days against the 60-day limit). Lesson: the clock is a rule, not a target.

## Try this

Write the four-factor memo for INC-2026-027. One page: facts and timeline, exception analysis, each factor in two or three lines, evidence needed (chat vendor deletion confirmation, retention settings, access logs), conclusion, and who decides. Then list what must change so a support engineer cannot paste PHI into the chat tool again (hint: DLP rule, ticket-redaction tooling, a BAA tier).
