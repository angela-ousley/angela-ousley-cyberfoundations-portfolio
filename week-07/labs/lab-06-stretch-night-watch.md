# Week 7 Lab 06 — Night Watch — Optional Stretch

*The Logbook*

**Student Name:** Angela Ousley

**Date Completed:** August 31, 2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 7  
**Submission Path:** `week-07/labs/lab-06-stretch-night-watch.md`

> ## Cloud Heights Protected-Rules Safety Rule
> Four baseline rules are protected: **100** (`allow-ssh-from-bastion`), **110** (`allow-icmp-intra-vnet`), **120** (`deny-ssh-student-subnet`), and **1000** (`deny-tcp8080-student-subnet` — Inbound Deny TCP from `10.60.6.0/26` to port `8080`). **You never modify, delete, replace, or use a protected rule as a troubleshooting target.** Create or edit student rules only in priorities **200–999**. The priority **1000** fallback deny sits after your band on purpose, so a narrower Allow you create in 200–999 is evaluated first. A mistake in your student range is recoverable and is not a grading penalty when you diagnose it honestly.

> **Evidence safety:** Never include a Cloud Heights password or Bastion shareable URL. Crop browser address bars and login information before committing screenshots.

---

## Mission

Turn the evidence already exposed by the Lab Portal into a short analyst logbook. The current student Portal does not expose a VNet flow-log viewer, so this lab does not ask you to locate or interpret one. Work only with visible security-rule state and **Test My Rule** results.

**Optional stretch:** Skipping this lab does not reduce your grade.

## What You Already Know

A network security rule is a decision about traffic. Rules are evaluated from the lowest priority number to the highest, and the first matching rule wins. Inbound and outbound traffic use separate ledgers. A configured service, a security rule, a test result, and an evidence screenshot answer different questions.

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Evidence available | Security Rules list and Test My Rule result cards |
| Not available | Student-facing VNet/NSG flow-log viewer |
| Change level | No new rule changes required |
| Time | 20–30 minutes; optional |

- [x] I am using my assigned `cf-student-XX` VM through the CyberFoundations Lab Portal.

- [x] The VM shows **Running**.

- [x] I can identify the four protected baseline rules at priorities 100, 110, 120, and 1000.

- [x] I understand that my editable priority range is 200–999.

### Cloud Heights Idle Stop

Cloud Heights may warn you that the VM is idle. Return to the Lab Portal and choose **I'm still working** if you are active. If the VM is stopped or deallocated, it was not deleted: restart it from **My Lab Environment**. Your disk files and saved configuration remain.

## Predict First

Predict which conclusion can be supported by each artifact: a rule screenshot, an `ALLOWED` result, a `DENIED` result, and the Python listener output.

```text
A rule screenshot shows configuration and is supporting evidence. An Allowed result confirms connectivity and is an enforcement result. A Denied result confirms unwanted traffic isn't allowed and is an enforcement result. The Python listener output confirms that the port is listening for a connection.
```

## Guided Steps

### Step 1 — Build the Evidence Chain

Use your Week 7 screenshots to identify:

1. **Configuration:** what the rule was designed to permit or deny.
2. **Enforcement result:** what the Portal test reported.
3. **Observed behavior:** whether the test source reached TCP 8080.
4. **Evidence artifact:** which screenshot supports the claim.

| Claim type | Claim | Supporting file | Limitation |
| --- | --- | --- | --- |
| Configuration | 250 Deny rule for inbound traffic from the Grid Beacon | week07-lab05-broken-rules.png | Only shows what's intended to happen. |
| Enforcement | The NSG decided the traffic was Denied | week07-lab05-observed-denial.png | Only shows part of the story, either Allowed or Denied |
| Observed behavior | Test source reached TCP 8080, but wasn't allowed | week07-lab05-observed-denial.png | Denied result shows that the source did reach TCP 8080, but can't be sure if Denial was b/c of the rule or the VM was unresponsive. |

### Step 2 — Write a Night-Watch Entry

**Date/time recorded by student:** N/A

**VM identifier:** 10.60.6.35

**Rule reviewed:** Rule 250 deny-grid-beacon-8080-test

**Intended source and port:** 10.60.6.4 any port

**Allowed-source result:** DENIED

**Unintended-source result:** N/A

**Conclusion:**

```text
Although traffic is Allowed via rule 300 for inbound traffic from the Grid Beacon 10.60.6.4, the Deny rule of 250 is not allowing that traffic to make it to my VM. I have a screenshot to support my conclusion, but I'm currently not able to provide proof of the time, date, and packet exchange that happened during testing because I don't have access to my virtual network log.
```

**Evidence filenames:** week07-lab06-evidence-set.png

### Step 3 — Name the Visibility Gap

Explain what you cannot conclude without a student-facing flow-log or audit viewer. Do not invent timestamps, packet records, or traffic history that the Portal does not show.

```text
Without a student-facing flow log I have no idea what time or date that the Denied result happened. I have no proof of the flow of traffic or packets traveling. If my only evidence of the Denied result is only from screenshot, I can't conclude whether the Denied result was because of rule 250 or if my VM just wasn't responding at the time. I might know that I tested my Deny rule at 1:00 pm, but if I can't show that then there's a gap in being able to provide proof. The log would show the source, destination, action, ports, and traffic direction along with the time and date so I could compare the log to the time that I ran my test and confirm my Deny result came from my rule based on the time a connection was made to my VM.
```

## Stop & Check

A rule list shows intended configuration. A test result shows one controlled test outcome. Neither is a complete history of all traffic.

## Test

No new test is required. If you repeat tests, keep the listener running, wait at least 10 seconds between tests, and use only the two Portal-provided sources.

## Capture Evidence

Create one clearly organized evidence-set screenshot or use the exact existing filenames from Labs 03–05. Do not fabricate a flow-log screenshot.

![Evidence set — week07-lab06-evidence-set.png](https://raw.githubusercontent.com/angela-ousley/angela-ousley-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-07/week07-lab06-evidence-set.png)

## Explain

Write an analyst-style conclusion that clearly separates what is proven, what is inferred, and what remains unknown.

```text
Based on the 'week07-lab06-evidence-set.png' screenshot, the configuration of rule 250 is proven. Getting a DENIED result from trying to connect via Grid Beacon 10.60.6.4 to my VM is also proven by the screenshot. What is inferred is that the Denial happened because of rule 250 and that traffic wasn't DENIED simply because my VM was unreachable. What remains unknown is the date and time in which a connection was made and when the denial result happened. For a more thorough analysis, a network flow log is needed.
```

## Required Evidence

Save screenshots in `assets/screenshots/week-07/`:

- `week07-lab06-evidence-set.png`

Open each image at full size before submission. Confirm that no password, Bastion shareable URL, browser address bar, or unrelated private information is visible.

## Analysis Questions

**Analysis Question 1.** How is a security rule different from a record of traffic that actually occurred? (Minimum 3 sentences.)

```text
A security rule is different from a record of traffic that actually occurred because a security rule is a plan on what you want to happen. A record of the traffic that occurred is proof of what actually happened. The record or log can be used to verify that a rule or control is working as intended or is failing.
```

**Analysis Question 2.** What can a single Test My Rule result prove, and what can it not prove? (Minimum 4 sentences.)

```text
A single Test My Rule result can prove that an allow or deny rule is working. If an allow rule gets tested and it passes, it confirms connectivity is working. A single test result for one requirement doesn't prove the result of a different requirement. If you test a deny rule and get a Denied test result, that confirms that unwanted traffic is being blocked, but it can't confirm that a separate allow rule is working. You have to show the results for a positive (allow) and a negative (deny) test to prove each rule is functioning as intended.
```

**Analysis Question 3.** Why is naming a visibility gap more professional than filling it with an assumption? (Minimum 3 sentences.)

```text
Naming a visibility gap is more professional that filling it with an assumption because it takes accountability of the gap. Making an assumption is just a guess and you never want to guess in security work. Naming the gap will allow you to track it, work around it, and possibly fix it in the future rather than forgetting about it or assuming that no one will notice it or allowing it to be exploited.
```

## Submission Checklist

- [x] Configuration, enforcement, observed behavior, and evidence distinguished

- [x] Night-watch entry completed

- [x] Portal visibility gap stated accurately

- [x] No flow-log viewer or traffic history invented

- [x] Evidence filenames verified

- [x] Protected priorities 100, 110, 120, and 1000 were not changed.

- [x] Every rule I created or edited used priority 200–999.

- [x] No password, Bastion URL, or browser address bar appears in my files.

- [x] This worksheet is committed to `week-07/labs/lab-06-stretch-night-watch.md`.

## GitHub / Lab Portal Submission

1. Open **Week 7 → Lab 06: Night Watch — Optional Stretch** in the CyberFoundations Lab Portal.
2. Complete every worksheet field and confirm the listed evidence filenames.
3. Upload screenshots to `assets/screenshots/week-07/`.
4. Confirm your portfolio repository is connected, then choose **Submit to GitHub**.
5. Open the committed worksheet and each image on GitHub to verify formatting, legibility, and redaction.

*CyberVisionaries Institute · CyberFoundations · Tier I*
