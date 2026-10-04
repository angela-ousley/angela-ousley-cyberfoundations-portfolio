# Week 10 Notes — Security Fundamentals and Risk

**Student Name:** Angela Ousley

**Date:** October 3, 2026

Use your own words and everyday examples. These notes support your thinking; they are not your lab answers. Do not copy clinic scenario answers here — those belong in the Demo Lab.

## 1. Assets and Business Purpose

An asset is something a business depends on. Pick an everyday example (a bakery, a gym, a school).

```text
My everyday business: A remote online notarization business
One asset it depends on: A laptop
Why the business needs that asset: An online notary needs a laptop in order to notarize documents via camera
```

## 2. Confidentiality, Integrity, Availability

Define each goal in your own words, then describe one situation where more than one goal is affected and explain why.

```text
Confidentiality means: Confidentiality means that only authorized users can access certain data that's being kept private.
Integrity means: Integrity means there aren't any changes to data, by mistake or malicious. Any changes will be tracked in a log or has some form of proof.
Availability means: Availability means that systems and data are ready to use when authorized users need access to it.
A situation where goals overlap, and why: Someone steals the laptop of the online notary. If the laptop isn't encrypted, who ever stole the laptop might be able to access the notarization records. If the application that processes the notarizations is signed in, the thief might be able to edit the records. If the notary doesn't have another computer or a backup of the records, then operations will come to a halt. Confidentiality is gone because the data is exposed to an outside party, integrity is lost if they can edit the records, and the notarization system and data are no longer available.
```

## 3. Event, Weakness, Consequence and Risk

Use your OWN non-clinic example to separate these four ideas.

```text
My example setting: An online notary leaves her laptop open in a public library while going to the bathroom.
Threat / event (what could happen): Her laptop is stolen.
Vulnerability (the weakness that lets it cause harm): Leaving her laptop unattended and unlocked.
Consequence (what goes wrong if it happens): Her clients records are now exposed to an unauthorized individual. They might edit information or sell the data on the dark web. The notary loses the trust of her clients.
Risk (how the pieces combine into something to manage): I rate this as 9- high risk. Even though she may not do that all of the time (little exposure), there's no protection for the laptop when she leaves it to go to the bathroom. The impact will be huge because operations will stop, sensitive data is exposed, and recovery is unknown because we don't know if she has another laptop or backup records.
```

## 4. Observed, Inferred and Unknown

Evidence you saw is different from a guess. If you did not see a safeguard, it is unknown — not proof it is missing.

```text
Something I observed directly: From a library recording, I observed the notary go into the bathroom and leave her laptop at a desk unattended. Another person got up from a nearby desk, picked up her laptop, and walked out of the library.
Something I inferred from it: I inferred that leaving the laptop alone was a risky move and that she didn't know the person that took her laptop.
Something that is still unknown: It's unknown if the computer was unlocked when she stepped away or if any records were visible. If the records on the computer are encrypted. What the thief will do with the information on the computer if they are able to access it.
How I will label a safeguard I did not see: I will label it as 'unknown'.
```

## 5. Warning Signs and Safe Reporting

Suspicious is not the same as proven.

```text
Warning signs I would look for: For a suspicious email, I would look for mismatched sender addresses, a false sense of urgency, generic greetings, and unexpected links or attachments.
How I would safely verify without clicking or replying: I'd check if the sender's display name matches their actual email domain. Put my mouse cursor over the link without clicking it to preview the actual destination URL. I wouldn't open any attachments.
Who I would report to, and how: I'd report it to the SOC team via a ticketing system or the security operations center email.
Why suspicion alone does not prove compromise: Suspicion alone does not prove compromise because there's a chance that everyone that came in contact with the phishing email did not click on anything or replay to the email.
```

## 6. Likelihood and Impact

Likelihood and impact are each rated 1–3. The classroom score is Likelihood x Impact. Bands: 1–2 low, 3–4 medium, 6–9 high. These are classroom judgments, not measured probabilities.

```text
What a likelihood of 1, 2 or 3 means to me: 1-slightly likely to happen or rarely happens at all
2-likely to happen or happens sometimes
3-very likely to happen or happens most of the time
What an impact of 1, 2 or 3 means to me: 1-low impact; operations go on as normal
2-medium impact; operations are impacted, but work arounds are in place
3-high impact; operations stop
Existing controls vs proposed controls, in my words: Existing controls are controls already in place by the company to prevent or mitigate issues. Proposed controls are what is suggested by security professionals.
Why recording my reason matters more than the number: Recording my reason matters more than the number because I need to show evidence as to why I'm giving it a rating.
```

## 7. Controls and Residual Risk

Use a non-clinic example.

```text
A specific control: My credit card company added multi-factor authentication in order to sign on into the app
How it helps: It helps by adding something I have along with something I know in order to authenticate me, so there are 2 controls instead of one.
Risk remaining afterwards: The risk remaining afterwards is that someone can steal my phone and if they already know my password, now they could access my credit card information. Or they can do SMS interception somehow and get access to my code.
```

## 8. Talking to a Manager

```text
How I would explain a risk in plain language to a non-technical manager: I would explain a risk as the potential harm that can happen to the company if certain scenarios come into play.
```

## 9. Questions and Terms to Revisit

```text
Terms I want to review: Out-of-Band verification: checking request through a medium you already know and trust.
Questions for my instructor: None at the moment.
```
