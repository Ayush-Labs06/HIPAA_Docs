# Pixels, analytics and tracking technologies on health websites

A tracking pixel is a snippet of code on a web page that reports what the visitor does to a third party (an ad network or analytics company). On a health site, "what the visitor does" can reveal that a person looked for a cardiologist, paid a bill at a named clinic, or logged in to a patient portal. That can be PHI going to a company with no business associate agreement (BAA).

## The rule in plain words

PHI is health information plus an identifier, held by a covered entity (CE) or business associate (BA). On a website, identifiers include name, email, IP address, device IDs, cookie IDs and account numbers. If a CE's or BA's page sends such an identifier together with health-related information to a third party, that is a **disclosure of PHI**. It needs a permission (usually none applies to advertising) or a BAA that allows the vendor's role. See [what_is_phi.md](../02_PHI_and_Data_Mapping/what_is_phi.md).

## OCR guidance and the court fight

| Date | Event | Status as of November 2026 |
| --- | --- | --- |
| 1 Dec 2022 | OCR Bulletin: "Use of Online Tracking Technologies by HIPAA Covered Entities and Business Associates" | Partly in force |
| 18 Mar 2024 | OCR updated the bulletin with examples and a FAQ | Partly in force |
| 20 Jun 2024 | *American Hospital Association v. Becerra*, N.D. Tex., No. 4:23-cv-01110-P: the court **vacated** the bulletin's statement about unauthenticated pages. It held OCR exceeded its authority by saying an IP address plus a visit to an unauthenticated public page is IIHI (the "Proscribed Combination") | Vacated on that point. The court denied a wider injunction. HIPAA Journal reports OCR withdrew its appeal; **check current status** for the latest on any further litigation |

What stayed standing, per law-firm summaries of the ruling:

- **Authenticated pages**: patient portals, bill-pay after login, telehealth sessions, symptom checkers where the user is logged in. Tracking there is high risk.
- **Mobile apps** that collect health data tied to identifiers.
- **Unauthenticated pages where the visit shows an intent to get care**, such as a form to book an appointment where the user types a condition and contact details. The court's ruling focused on pages that only show interest, not on those that gather identity and a care request, but this edge is unsettled.

Do not read the vacatur as "pixels are fine". Three reasons:

1. Tracking on login and portal pages was never covered by the vacatur.
2. The Security Rule, BAA terms and customer contracts still apply.
3. **Courts and state law do not depend on OCR's bulletin.** Class actions use wiretap, privacy and consumer-protection claims; the FTC uses Section 5; state laws such as Washington's My Health My Data Act apply separately ([health_apps_and_non_hipaa_data.md](./health_apps_and_non_hipaa_data.md)).

## Real enforcement and litigation

| Matter | Year | Amount | What happened | Lesson |
| --- | --- | --- | --- | --- |
| **Advocate Aurora Health** pixel litigation (E.D. Wis.) | Settled 2023, final approval 2024 | **US$12.225 million** common fund | Meta pixel and similar tools on its websites, LiveWell app and MyChart portal sent data to third parties (class period Oct 2017 to Oct 2022). No admission of wrongdoing | Portal and app tracking = large class actions |
| **Novant Health** MyChart pixel litigation | 2023-2024 | Reported fund of about **US$6.6 million** (check amount) | Pixel on a patient portal/scheduling tool; Novant said no wrongdoing was found | Same pattern, plus a patient notification |
| **FTC v. GoodRx** | 2023 | **US$1.5 million** civil penalty | First action under the FTC Health Breach Notification Rule; shared health data with ad platforms. Permanent ban on sharing health data for advertising | A non-HIPAA company; see the next file |
| **State AG actions** | Various | Varies | Several state attorneys general have investigated health sites' trackers | **Check current status**; I could not verify a specific action to cite |

Settlements are not findings of liability. They show what plaintiffs, courts and regulators think is expensive.

## PulseConnect: what to do on `pay.pulseconnect.example`

PulseConnect is a BA to its provider customers. The bill-pay portal, including the **login page and the authenticated pages**, loads a **Meta Pixel and Google Analytics**. That is the as-is state.

Decisions, in order:

1. **Remove third-party marketing pixels** from the login and every authenticated page. No exception. These pages show who is paying which provider.
2. **Marketing site** (`pulseledger.example`) is PulseLedger's own site, not PHI, but keep it separate: a different tag container, no cross-domain identifiers.
3. **Analytics you truly need**: use a vendor that will sign a BAA, or self-host (a first-party analytics tool on your own AWS account). If a vendor will not sign a BAA, it cannot receive PHI. As of this writing, major ad platforms generally do not sign BAAs; check each vendor's current terms.
4. **Consent is not a fix** for PHI going to a vendor without a BAA. A cookie banner does not create a permission under the Privacy Rule. A HIPAA authorization is a different, stricter instrument ([authorizations_and_special_permissions.md](../03_Privacy_Rule/authorizations_and_special_permissions.md)). Where the page belongs to a customer CE, the customer decides; PulseLedger must not add trackers for its own marketing.
5. **Server-side tagging cautions**: moving a pixel to your server does not change what is sent. If the server forwards user IDs, IPs or page names to an ad platform, it is still a disclosure. Strip identifiers, or do not forward.
6. **Tell the CEs**: if PHI already went to the vendor, run the four-factor assessment ([is_it_a_breach.md](../07_Breaches_and_Incidents/is_it_a_breach.md)) and notify customers per their BAAs ([breach_notification_60_days.md](../07_Breaches_and_Incidents/breach_notification_60_days.md)).

## Audit method (observe only)

Do this only on sites you own or on public pages you view as an ordinary visitor. Never log in to someone else's portal or enter data.

1. **Open DevTools, Network tab**, tick "Preserve log" and "Disable cache". Load the page. Filter by `Third-party`.
2. **Export a HAR** (Save all as HAR). Search it for the domains below and for parameters that look like identifiers (`email`, `uid`, `fbp`, `_ga`, `cid`).
3. **Inspect payloads**: does the request include the page URL, page title, form field names, or button labels? URLs such as `/pay/invoice?clinic=riverbend-cardiology` leak context.
4. **Tag inventory**: list every script by domain, owner, purpose, data sent, BAA yes/no, page scope.
5. **Repeat** on the logged-in test account that you own (login page, dashboard, payment page).
6. **Repeat with privacy tools off and on** to see whether it changes.

### Sample tag list for `pay.pulseconnect.example` (fictional)

| Page | Tag | Vendor type | Data seen | BAA | Action |
| --- | --- | --- | --- | --- | --- |
| Login | Meta Pixel | Ad network | IP, URL, cookie ID | No | **Remove** |
| Login | Google Analytics | Web analytics | IP, URL, client ID | No | **Remove** or replace |
| Dashboard (authenticated) | Meta Pixel | Ad network | Page title "Your balance", cookie ID | No | **Remove** |
| Pay invoice | Google Tag Manager | Tag container | Loads other tags | No | Remove from this scope |
| Pay invoice | Session-replay script | Third party | Form field values | No | **Remove** |
| Help centre | Chat widget | Support vendor | Typed text, email | Yes | Keep; confirm BAA covers it |
| Static assets | CDN | Delivery | IP | Yes (AWS) | Keep |

## Try this

Audit the fictional tag list above. For each row decide: keep, replace or remove, and write one sentence on why. Then write the PHI/not-PHI call for the "Help centre" chat widget and name the contract clause you would check. Last, list the data you would need from AWS (CloudFront logs, WAF logs) to estimate how many patients' data went to the vendor since launch.
