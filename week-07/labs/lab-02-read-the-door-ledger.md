# Week 7 Lab 02 — Read the Door Ledger

**Student Name:** Angela Ousley

**Date Completed:** Aug 27, 2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 7  
**Submission Path:** `week-07/labs/lab-02-read-the-door-ledger.md`

> ## Cloud Heights Protected-Rules Safety Rule
> The baseline rules at priorities **100** (`allow-ssh-from-bastion`), **110** (`allow-icmp-intra-vnet`), and **120** (`deny-ssh-student-subnet`) are protected. **Never modify, delete, replace, or use them as troubleshooting targets.** Create or edit student rules only in priorities **200–999**. A mistake in your student range is recoverable and is not a grading penalty when you diagnose it honestly.

> **Evidence safety:** Never include a Cloud Heights password or Bastion shareable URL. Crop browser address bars and login information before committing screenshots.

---

## Mission

Learn to read inbound and outbound rule ledgers in evaluation order. Translate a rule from field values into plain English, then predict which matching rule makes the decision.

## What You Already Know

A network security rule is a decision about traffic. Rules are evaluated from the lowest priority number to the highest, and the first matching rule wins. Inbound and outbound traffic use separate ledgers. A configured service, a security rule, a test result, and an evidence screenshot answer different questions.

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Cloud Heights → Security Rules |
| Change level | Read-only reasoning |
| Evaluation | Lower priority number first; first match wins |
| Time | 25–30 minutes |

- [x] I am using my assigned `cf-student-XX` VM through the CyberFoundations Lab Portal.

- [x] The VM shows **Running**.

- [x] I can identify the three protected baseline rules at priorities 100, 110, and 120.

- [x] I understand that my editable priority range is 200–999.

### Cloud Heights Idle Stop

Cloud Heights may warn you that the VM is idle. Return to the Lab Portal and choose **I'm still working** if you are active. If the VM is stopped or deallocated, it was not deleted: restart it from **My Lab Environment**. Your disk files and saved configuration remain.

## Predict First

Two inbound rules match TCP 8080 from the same source: priority 250 is **Deny** and priority 300 is **Allow**. Predict the verdict before reading further.

```text
I predict that the inbound traffic from TCP 8080 will be denied. The 250 rule of deny will process first, so the 300 rule of allow will not even be considered.
```

## Guided Steps

### Step 1 — Separate the Ledgers

View the inbound rules, then the outbound rules. Record one sentence explaining why an inbound allow does not automatically create an outbound allow.

```text
An inbound allow does not automatically create an outbound allow because the ledgers are separate and each ledger has different rules.
```

### Step 2 — Translate a Rule

Choose one visible protected rule and translate it using this form:

> Read in the [direction] ledger at priority [number], [allow/deny] [protocol] traffic from [source]:[source port] to [destination]:[destination port].

```text
Read in the inbound ledger at priority 120, deny TCP traffic from 10.60.6.0/26:* to 10.60.6.35:22.
```

### Step 3 — Evaluate in Order

For each scenario, list the rules in evaluation order, identify the first match, and state the verdict.

1. Priority 250 Deny TCP from `10.60.6.4` to port 8080; priority 300 Allow the same traffic.
2. Priority 300 Allow TCP from `10.60.6.4` to port 8080; priority 350 Deny TCP from any source to port 8080.
3. An inbound Allow exists, but the traffic being evaluated is outbound.

```text
1. Rule 250 is the first match. Inbound TCP traffic from 10.60.6.4 to 10.60.6.35:8080 would be denied.
2. Rule 300 is the first match. Inbound TCP traffic from 10.60.6.4 to 10.60.6.35:8080 would be allowed.
3. The inbound allow rule doesn't matter because the outbound traffic has a separate ledger.
```

## Stop & Check

If you find yourself reading the Allow you want and ignoring an earlier matching Deny, restart at the lowest priority number. The ledger stops at the first match.

## Test

Use the displayed rule list to predict whether Grid Beacon TCP 8080 would currently be explicitly allowed by a student rule. Do not press **Test My Rule** yet unless your instructor directs you; the service may not be listening.

## Capture Evidence

Capture the rule view in its displayed evaluation order and annotate your worksheet with the first rule you would inspect for each scenario.

![Rule view in evaluation order — week07-lab02-evaluation-order.png](https://raw.githubusercontent.com/angela-ousley/angela-ousley-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-07/week07-lab02-evaluation-order.png)

## Explain

Write a five-sentence explanation of first-match-wins that a classmate could use without memorizing Azure terminology.

```text
When it comes to security rules, there's an evaluation order. The 'first-match-wins' is the easiest way to look at it. If you have 3 rules and each rule is labeled with a priority number, the lowest number will be checked (matched) first. So rule 100 would be checked before rule 200, rule 250 would be check before rule 300, and so on. Just remember the lower numbers get prioritized first and the rules after the initial match don't get checked.
```

## Required Evidence

Save screenshots in `assets/screenshots/week-07/`:

- `week07-lab02-evaluation-order.png`

Open each image at full size before submission. Confirm that no password, Bastion shareable URL, browser address bar, or unrelated private information is visible.

## Analysis Questions

**Analysis Question 1.** If a Deny at priority 300 and an Allow at priority 400 both match, which wins and why? (Minimum 3 sentences.)

```text
If a 'deny' is at priority 300 and an 'allow' is at priority 400, the 300 deny wins. The 300 wins because it comes before 400, the first match wins. Rule 400 will not be evaluated.
```

**Analysis Question 2.** Why do inbound and outbound rules have to be reasoned about separately? (Minimum 3 sentences.)

```text
Inbound and outbound rules are separated because inbound/outbound traffic carry different security risk and need to be evaluated accordingly. Outbound traffic to a specific IP address and port might be allowed with an outbound rule, but incoming traffic from that same IP address might be blocked with an inbound rule. Having separate ledgers makes the rules easier to view, follow, and edit.
```

**Analysis Question 3.** How can an Allow rule be correct by itself but ineffective in the full ledger? (Minimum 3 sentences.)

```text
An 'allow' rule can be correct by itself, but ineffective in the full ledger because its priority number comes after a rule that's more important. For example, if rule 210 says to 'deny' inbound traffic from 192.168.10.128/26, but rule 350 says to 'allow' inbound traffic from that same IP, rule 210 would match first, so rule 350 wouldn't even be considered. Always start at the lowest priority number.
```

## Submission Checklist

- [x] Three scenarios evaluated in order

- [x] One live rule translated to plain English

- [x] Inbound and outbound ledgers distinguished

- [x] `week07-lab02-evaluation-order.png` captured

- [x] Protected priorities 100, 110, and 120 were not changed.

- [ ] Every rule I created or edited used priority 200–999.

- [x] No password, Bastion URL, or browser address bar appears in my files.

- [x] This worksheet is committed to `week-07/labs/lab-02-read-the-door-ledger.md`.

## GitHub / Lab Portal Submission

1. Open **Week 7 → Lab 02: Read the Door Ledger** in the CyberFoundations Lab Portal.
2. Complete every worksheet field and confirm the listed evidence filenames.
3. Upload screenshots to `assets/screenshots/week-07/`.
4. Confirm your portfolio repository is connected, then choose **Submit to GitHub**.
5. Open the committed worksheet and each image on GitHub to verify formatting, legibility, and redaction.

*CyberVisionaries Institute · CyberFoundations · Tier I*
