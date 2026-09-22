# Week 9 Lab 01 - Investigate a Certificate

**Student Name:** Angela Ousley

**Date Completed:** September 22, 2026

**Module:** 3 - Practical Cryptography | **Week:** 9  
**Submission Path:** `week-09/labs/lab-01-investigate-a-certificate.md`

> ## Vault Exchange Trust and Key Safety Rule
> Inspect a public website without signing in. Do not click past a browser certificate warning, add a practice issuing office (CA) to your browser’s accepted list, or upload secret private keys. Keep all Week 6-8 VM files, SSH keys, and access settings unchanged. This lab makes no security-rule changes.
>
> **Evidence safety:** Capture the certificate viewer, not your account, bookmarks, browser address bar, passwords, or Bastion access URL. Record the public hostname as text in this worksheet.

---

## Mission

Ivy has two digital keys. Both say “Vault Exchange Support.” How can she tell which one belongs to the support team? A label alone is not enough.

Think of a visitor showing a badge at a front desk. The guard checks the name, the dates, and the office that issued it. In this lab, you will look at a website’s digital badge, called a **certificate**. You will write down what you see and follow the list of offices that signed it. You are looking and recording; you are not changing the website.

A visitor badge has a name, issuing office, validity period, and permitted use. A certificate similarly supplies information to check. A polished badge does not make its issuer trusted, and a certificate does not guarantee that a website's advice or downloads are safe.

## What You Already Know

You do not need to memorize last week’s vocabulary. Use these reminders:

- A **public key** is the shareable part of a pair of digital keys. Its label alone does not prove who owns it.
- A **private key** is the secret part. Keep it private, like a key to a locked room.
- A **digital signature** is a mathematical check made with a private key. It helps detect changes and check which matching key signed something.
- A **browser** is the app you use to visit websites. It checks certificates for you.
- A **hostname** is a website’s name, such as `example.com`. It does not include `https://` or the page name after the slash.

Our question is: “Does this digital badge fit the website I meant to visit, and does my browser accept the office behind it?”

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Your normal desktop browser; assigned VM is not needed for this investigation |
| Public target | Start with `https://example.com`; use an instructor-approved public HTTPS site if unavailable |
| Change level | Read-only inspection; no sign-in, certificate import, or warning bypass |
| Time | 35-50 minutes; pause and resume as needed |
| Evidence | Certificate fields, browser chain view, dated observations, and your explanation |

- [x] I have watched Lessons 1-3 or reviewed their slides.

- [x] I can open the public site without signing in.

- [x] I know that live issuer names and dates may differ from the lesson images.

- [x] I will write “not observed” when the viewer does not expose a field.

### Cloud Heights Idle Stop

This browser investigation can be completed while your VM is stopped. If you also open Cloud Heights, respond to its idle warning only while actively working. Restart a stopped VM from My Lab Environment when you need it; do not rebuild it.

## Predict First

A visitor’s badge has not expired. Is checking the date enough to let that visitor in? What else should the guard check? Connect your prediction to a website’s certificate in 2–3 sentences. A prediction is your best guess before investigating; it does not have to be correct.

```text
No, checking the date is not enough to let the visitor in. The guard should check for who issued the badge, the identity of the person trying to enter, and what permissions they have. The badge needs to be verified. A website's certificate is checked in a similar way; who issued the certificate, subject alternative name (SAN), validity period, key usage, and more.
```

## Guided Steps

### Step 1 - Identify the Destination and Observation

1. Open your browser and visit `https://example.com`. Do not sign in or enter a password.
2. Look at the address after the page loads. Sometimes one address sends you to another; that is a **redirect**. Write the name you ended up visiting in the table.
3. Record today’s date, time, and time zone. A time zone tells us which local clock you used.
4. Open the site-information button beside the address. Look for connection or certificate information. Wording may include “Connection is secure” or “Certificate is valid.”
5. Open the certificate details. Browser menus differ. If you cannot find them, ask your instructor to show you. Do not install anything.

**What you should see:** information about the website’s certificate. Copy the browser’s message exactly. For the browser version, use its Help/About screen if available; ask for help if needed.

| Observation | Your record |
| --- | --- |
| Starting public URL | https://example.com/ |
| Final expected hostname | example.com |
| Observation date and time | September 21, 2026 at 5:25 PM |
| Time zone | Eastern Daylight Time (EDT) |
| Browser and version | Chrome Version 116.0.5845.187 (Official Build) (x86_64) |
| Browser connection/certificate status, exactly as shown | 'Connection is secure' |

If a warning appears, stop before proceeding to the site. Record the warning privately and select an approved alternative with your instructor. A warning is not a request to disable verification.

### Step 2 - Read the Leaf Certificate

Select the certificate for the website itself. This is called the **leaf certificate** because it is at the end of the signing chain. Other certificates in the list belong to the offices that issued certificates.

Open Details or Fields. A **field** is one labeled piece of information, like “Name” on a badge. Work down the table one row at a time. Use this guide to understand the labels:

| Label in the viewer | Plain-language meaning |
|---|---|
| Subject | Who or what this certificate describes. |
| Subject Alternative Name (SAN) | The list of website names the certificate covers. Use this list to check the name you visited. |
| Issuer | The office that signed this certificate. Such an office is called a certificate authority, or **CA**. |
| Not Before / Not After | The start and end of the certificate’s allowed date-and-time period. |
| Subject public-key algorithm and size | The kind of public key and its size. An **algorithm** is a set of instructions for doing a calculation. Copy the label and number you see. |
| Certificate signature algorithm | The method the issuing office used to sign the certificate. This is a different job from the website’s own public key. |
| Extended Key Usage (EKU) | The listed jobs for this certificate, such as identifying a web server. |
| Basic Constraints | Whether this certificate may act as an issuing office (CA). |
| Serial number / SHA-256 fingerprint | A tracking number, or a calculated fingerprint, that helps identify the particular certificate you inspected. |

You do not need to explain how the algorithms work. In the last column below, explain the field’s job in your own words. Expand a long list to see its entries. A Subject “Common Name” is not a substitute for checking the SAN list.

| Field | Value you observed | What question does this field help answer? |
| --- | --- | --- |
| Subject | CN = example.com | Common name of the website |
| Subject Alternative Name (SAN) | DNS Name: example.com DNS Name: *.example.com | Other websites that the certificate covers |
| Issuer | CN = Cloudflare TLS Issuing ECC CA 3 O = SSL Corporation C = US | Who signed the certificate |
| Not Before | 7/29/26, 6:10:08 PM EDT | Cert isn't valid before this date |
| Not After | 10/27/26, 6:17:21 PM EDT | Cert isn't valid after this date |
| Subject public-key algorithm and size, if shown | Elliptic Curve Public Key, size not observed | What type of public key and the size of it |
| Certificate signature algorithm | X9.62 ECDSA Signature with SHA-256 | Algorithm used to sign the certificate |
| Extended Key Usage (EKU), if shown | TLS WWW Server Authentication (OID.1.3.6.1.5.5.7.3.1) | What the cert is allowed to do |
| Basic Constraints, if shown | Is not a Certification Authority | If the certificate can issue certificates |
| Serial number or SHA-256 certificate fingerprint | 61 53 A9 6F D1 A6 AB 7F 4D 43 8F C3 49 32 48 42 99 D0 72 9D 91 40 B3 A1 26 BB 2F 9C 07 B0 22 00 | Hash value of SHA-256 function |

Copy the matching SAN entry fully. If other names are listed, say “additional names listed.” Keep the website’s public-key information separate from the method used to sign its certificate.

**If you cannot find a field:** write “not observed” and ask your instructor. If you have confirmed that the full certificate has no EKU field, write “extension not present.” Not seeing a label in a limited viewer does not prove it is missing from the certificate.

### Step 3 - Check the Website Name and Dates

Now do two badge checks yourself: “Right name?” and “Still within its dates?” A wildcard is a `*` that stands in for part of a name.

1. Compare your final expected hostname with the leaf's DNS SAN entries. For a wildcard, `*.example.com` ordinarily matches one label such as `shop.example.com`, not `example.com` or `a.shop.example.com`.
2. Compare the observation time with Not Before and Not After using consistent time zones. Do not change your device clock.
3. Record the browser result separately from the field comparison.

```text
Expected hostname: example.com
Matching SAN entry, or no match: Yes, one of the SAN entries matches my expected hostname (example.com)
Why the entry matches or does not match: The entry matches because my expected hostname and the first DNS Name both read as 'example.com'. The second DNS name doesn't match because '*.example.com' is different from 'example.com'.
Observation time and certificate time zone: My observation time is 5:25 PM EDT and the certificate time zone is in EDT.
Is your observation time between the start and end dates? Explain: Yes, my observation time is between the start and end dates. September 21, 2026 5:25 PM is between 7/29/26, 6:10:08 PM EDT and 10/27/26, 6:17:21 PM EDT.
Browser-reported result: 'Connection is secure'
What I checked myself versus what the browser reported: I checked the certificate hierarchy, validity period, the issuer of the certificate, basic constraints, key usage, SAN, and serial number from the leaf, intermediate, and root certificates within the browser.
```

### Step 4 - Map the Observed Chain

Look for a view called **hierarchy**, **certification path**, or **chain**. These are names for the list of certificates linked by signatures.

Think of a badge office approved by a larger office:

- **Leaf:** the website’s badge.
- **Intermediate:** an issuing office between the website and the top office. There can be more than one, or none shown.
- **Root / trust anchor:** the top office your browser is set up to accept. A **client** is the program doing the checking; here, that is your browser.

Record only what your browser shows. The browser may add a root from its own stored list. This screen is not a recording of everything the website sent. Also, reading matching office names is not the same as doing the mathematical signature checks yourself.

| Position / role | Subject | Issuer | Where observed | What remains unknown? |
| --- | --- | --- | --- | --- |
| Leaf / website | CN = example.com | CN = Cloudflare TLS Issuing ECC CA 3 O = SSL Corporation C = US | Certificate Hierarchy, under the Cloudflare TLS Issuing ECC CA 3 certificate | If this certificate has been revoked. If the signature value is correct. |
| Intermediate, if shown | CN = Cloudflare TLS Issuing ECC CA 3 O = SSL Corporation C = US | CN = SSL.com TLS Transit ECC CA R2 O = SSL Corporation C = US | Certificate Hierarchy, under the SSL.com TLS Transit ECC CA R2 certificate | If the signature value is correct. |
| Additional intermediate, if shown | CN = SSL.com TLS Transit ECC CA R2 O = SSL Corporation C = US | CN = SSL.com TLS ECC Root CA 2022 O = SSL Corporation C = US | Certificate Hierarchy, under the SSL.com TLS ECC Root CA 2022 certificate | If the signature value is correct. |
| Root / trust anchor, if shown | CN = SSL.com TLS ECC Root CA 2022 O = SSL Corporation C = US | CN = SSL.com TLS ECC Root CA 2022 O = SSL Corporation C = US | Certificate Hierarchy at the top | If the signature value is correct. |

Remove unused intermediate rows or mark them “not shown.” Do not invent a three-certificate chain. If the viewer exposes only the leaf, report that limit and ask the instructor for a supported viewer demonstration.

Make a simple labeled chain sketch, or write the relationship in words:

```text
The website leaf is signed by: Cloudflare TLS Issuing ECC CA 3, which is an intermediate certificate.
That issuer is signed by (if shown): SSL.com TLS Transit ECC CA R2, which is another intermediate certificate.
The top office (root / trust anchor) shown by my browser is (or not observed): SSL.com TLS ECC Root CA 2022 is the root certificate.
Why an office signing its own badge is not enough (who must choose to accept that office?): An office signing its own badge isn't enough, other people (clients) must choose to accept that office.
This view is the browser's displayed path; I have / have not independently captured what the server sent: This view is the browser's displayed path (Leaf > Intermediate > Intermediate > Root); I have not independently captured what the server sent.
```

## Stop & Check

- Is the selected certificate the website leaf, rather than its CA?
- Did you distinguish the subject key from the issuer's signature algorithm?
- Did you record the actual observation date and time zone?
- Did you label a field you could not see as “not observed”?
- Did you explain why an office signing its own badge does not make everyone accept that office?

## Test

Your test is to compare the name and dates, then record the browser’s result. Write “I checked…” for your own work and “The browser reported…” for its result. You have not separately checked every signature yourself. You also have not separately checked whether an issuer canceled the certificate early; that is called **revocation**. Do not claim checks you did not perform.

## Capture Evidence

Capture the expanded leaf fields and the chain/hierarchy view. Use additional numbered images when one image cannot show all fields legibly. Include captions describing what each screenshot proves. Keep account details and browser address bars outside the crop.

## Explain

Write 3–4 sentences using the visitor-badge example. Explain one field, who signed the website’s certificate, why the browser accepts the top office, and one thing a certificate cannot promise. Sentence starters: “The SAN list is like…”, “The issuing office…”, “My browser accepts…”, and “This does not mean…”.

```text
The serial number on a certificate is similar to an employee/visitor id number on a badge. The extended key usage states what the certificate is allowed to do, similar to a badge allowing you or not allowing you to enter into a specific building. The serial number on a certificate is similar to a visitor id number on a badge. Cloudflare TLS Issuing ECC CA 3 signed the certificate for 'example.com'. The browser accepts the top office because it has been configured/assigned to do so. A certificate can NOT promise that the links or data on a website is safe.
```

## Required Evidence

Save screenshots in `assets/screenshots/week-09/`:

- `week09-lab01-certificate-fields.png` (add `-02`, `-03` if needed)
- `week09-lab01-chain-view.png`

Complete all observation, field, name/time, and chain records in this worksheet. A chain sketch can be embedded as `week09-lab01-chain-map.png` or written in the provided fields. Browser-specific layouts and live certificate changes are acceptable when the evidence is internally consistent.

### Evidence Upload - Certificate Fields (required)

![week09-lab01-certificate-fields.png](https://raw.githubusercontent.com/angela-ousley/angela-ousley-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab01-certificate-fields.png)

**Caption - certificate fields:** The certificate fields screenshot shows the list of details about the certificate. Issuer, Validity, SAN, Key Usage, etc.

### Evidence Upload - Chain View (required)

![week09-lab01-chain-view.png](https://raw.githubusercontent.com/angela-ousley/angela-ousley-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-09/week09-lab01-chain-view.png)

**Caption - chain view:** The chain view screenshot shows the 4 different certificates and their hierarchy.

### Optional Extra Evidence (leave blank if you do not need it)

**Caption - extra certificate fields image:**

**Caption - third certificate fields image:**

**Caption - chain sketch image:**

## Analysis Questions

**Analysis Question 1.** Ivy has two working keys with the same label. Why does a working key or a correct signature not, by itself, prove that the key belongs to the support team? Write at least 3 sentences.

```text
A working key or a correct signature doesn't prove that they key belongs to the support team because anyone can label a key to what they decide to name it. This is shown by Ivy having two working keys with the same label. The public key working and the signature being correct only proves a cryptographic relationship between the public and private keys. Ivy will have to do more research by looking into the certificate associated with each key to determine which one belongs to the support team.
```

**Analysis Question 2.** Anyone could print an office name on a badge. Why must the browser check more than the printed issuer name? Explain how the browser’s settings or accepted list determines which top offices it trusts. Write at least 3 sentences.

```text
A browser must check more than the printed issuer name because a familiar issuer name can be added to the certificate, but it's not actually issued by who you think it is. The browser checks each certificate (root, intermediate(s), leaf) and checks all the fields associated with each certificate (Issuer, Validity Period, SAN, Key Usage, etc). The browser is configured to accept certain Trust Anchors, usually added by the Trust Store or an administrator.
```

**Analysis Question 3.** What did your name, date, and browser checks tell you about this connection? Why do they not promise that everything the website says or offers is safe? Say which checks you did and which you did not do. Write at least 3 sentences.

```text
My name, date, and browser checks tell me that the connection is secure. It does not promise that everything on the website is safe because it doesn't verify what's on the website, only information detailed on each certificate associated with the website. I checked the certificate hierarchy (root, intermediates, and leaf) and certificate fields, specifically the issuer, validity, certificate subject alternative name, extended key usage, and signature algorithm fields. I did not check if the signature values were correct on the certificates, if the certs were revoked, or if the hash (fingerprint) values were correct.
```

## Submission Checklist

- [x] Public hostname, observation time, time zone, and browser recorded

- [x] Certificate fields completed, with unavailable fields labeled honestly

- [x] SAN/name and validity comparisons explained

- [x] Observed chain roles mapped without inventing missing certificates

- [x] Browser-reported result distinguished from my own inspection

- [x] Both required screenshot subjects captured clearly

- [x] Analysis answers completed in my own words

- [x] No credentials, private keys, account details, or Bastion URL included

- [x] Worksheet saved to `week-09/labs/lab-01-investigate-a-certificate.md`

## GitHub / Lab Portal Submission

A **repository** is your project’s folder on GitHub. A **commit** is a saved set of changes. A `.md` file is a text document that uses simple formatting marks. Keep the headings and fill in the blank answers.

1. Complete this worksheet on this page, then press **Save Progress**. Your answers are stored in the portal and reload the next time you open this lab.
2. Press **Submit to GitHub**. The portal writes the finished worksheet to the submission path above in your connected portfolio repository. If you have not connected GitHub yet, the portal will ask you to connect and pick your repository first.
3. Add the reviewed screenshot addresses in the evidence fields above, or upload the images under `assets/screenshots/week-09/` in your portfolio repository.
4. Open the committed worksheet and every image on GitHub. Confirm that tables render, images are legible, and private information is absent.
5. Keep this investigation for Portfolio Deliverable 3 and Lab 02's comparison.

*CyberVisionaries Institute · CyberFoundations · Tier I*
