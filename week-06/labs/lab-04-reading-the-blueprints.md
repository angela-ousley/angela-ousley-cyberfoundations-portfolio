# Week 6 Lab 04 — Reading the Blueprints

**Student Name:** Angela Ousley

**Date Completed:** Aug 21, 2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 6  
**Submission Path:** `week-06/labs/lab-04-reading-the-blueprints.md`

---

> ### 🔒 Cloud Heights Security Rule
> Your Bastion link and Cloud Heights password are **private access credentials**. Never paste either into a worksheet, screenshot, GitHub repository, Circle post, or chat message. When taking screenshots, crop out the browser address bar and all login information.

---

## Overview

**This is a SHORT lab — 15 to 20 minutes.** It is deliberately small. You already have the commands; this lab is about matching a drawing to reality.

The **Cloud Heights Network Blueprint** is displayed at the top of this lab page in the portal. Everything you write about the network's architecture comes from that blueprint or from your own machine — never from a guess.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Cloud Heights — live Ubuntu 22.04 VM, reached through Azure Bastion |
| Source of truth | The Cloud Heights Network Blueprint shown at the top of this lab page |
| Commands used | `ip addr`, `ip route` |
| Known value | Student subnet: **`10.60.6.0/26`** |

---

## Part A — Read the Drawing

### Step 1 — Record the Architecture Values

From the blueprint at the top of this page, record each value **exactly as drawn**. If a value is not shown on the blueprint, write "not shown on blueprint" — do not guess.

| Item | Value from the blueprint |
| --- | --- |
| VNet name | CLOUD HEIGHTS VNET |
| VNet address space | 10.60.6.0 |
| Student subnet range | /24 |

---

## Part B — Verify Against Your Own Machine

### Step 1 — Confirm Your Address Lives in the Subnet

Run `ip addr` and find your private IPv4 address.

Command and output:

```
analyst@cf-student-16:~$ ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 7c:ed:8d:2a:77:21 brd ff:ff:ff:ff:ff:ff
    inet 10.60.6.35/26 metric 100 brd 10.60.6.63 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::7eed:8dff:fe2a:7721/64 scope link 
       valid_lft forever preferred_lft forever
3: enP569s1: <BROADCAST,MULTICAST,SLAVE,UP,LOWER_UP> mtu 1500 qdisc mq master eth0 state UP group default qlen 1000
    link/ether 7c:ed:8d:2a:77:21 brd ff:ff:ff:ff:ff:ff
    altname enP569p0s2
```

Your private IP:

```
10.60.6.35/26
```

Explain how you know your address falls inside `10.60.6.0/26` — what range does that prefix actually cover:

```
I know my address (10.60.6.35/26) falls inside of 10.60.6.0/26 because the /26 mask means that the first 3 octets (10.60.6) are for the network and a quarter of the 4th octet ( 2 bits) are also for the network. My first 3 octets are the same as the first 3 octets for the student subnet floor. The 2 network bits in the last octet leave 6 hosts bits to work with. 2^6 = 64 so that means there are 64 addresses on the student subnet floor ranging from 0 to 63. The range is 10.60.6.0 - 10.60.6.63. My host portion of my address ends in .35 which is in between .0 and .63 so my address falls inside the range.
```

### Step 2 — Confirm Route Behaviour

Run `ip route`.

Command and output:

```
analyst@cf-student-16:~$ ip route
default via 10.60.6.1 dev eth0 proto dhcp src 10.60.6.35 metric 100 
10.60.6.0/26 dev eth0 proto kernel scope link src 10.60.6.35 metric 100 
10.60.6.1 dev eth0 proto dhcp scope link src 10.60.6.35 metric 100 
168.63.129.16 via 10.60.6.1 dev eth0 proto dhcp src 10.60.6.35 metric 100 
169.254.169.254 via 10.60.6.1 dev eth0 proto dhcp src 10.60.6.35 metric 100 
```

What the default route tells you about traffic that is not destined for your own subnet:

```
The default route tells me that traffic that isn't destined for my subnet will leave through 10.60.6.1 because that's the gateway.
```

### Step 3 — Capture Your Evidence

**Required filename:** `blueprint-verified.png`

This must be **your own `ip addr` and `ip route` output** — not a re-screenshot of the blueprint. Crop out the address bar and any login information.

![Blueprint verified — my address inside the student subnet](https://raw.githubusercontent.com/angela-ousley/angela-ousley-cyberfoundations-portfolio/9d5b5d6c456f5425f80652434dee694734367567/assets/screenshots/week-06/blueprint-verified.png)

---

## Part C — How Traffic Actually Moves

### Step 1 — No Public IP

Your VM has a private address and **no public IP**. Explain what that means for who can reach it directly from the internet:

```
Since the VM has no public IP, that means no one can reach it directly from the internet. Azure Bastion acts as a guarded front desk and is the only way data can travel in from the internet.
```

### Step 2 — Outbound vs. Inbound

Outbound internet traffic from your VM leaves through address **translation (NAT)**. Inbound access for you arrives through **Azure Bastion**, not through a public address on the VM.

Explain both directions in your own words:

```
Outbound traffic leaves via NAT. NAT translates the private IP into a public IP address and sends the data along the internet. Inbound traffic comes through Azure Bastion as a response to the outbound request and it has to go through Azure since the VM doesn't have a public IP address.
```

### Step 3 — The Guard Post You Do Not Touch Yet

Each student machine sits behind its own **network security group** — a per-student guard post that decides what traffic is allowed in.

**In Week 6 you do not configure it.** Week 7 is when you take control of those rules.

Write one sentence naming what the guard post does and one sentence stating what you are *not* doing with it this week:

```
The guard post (Bastion) controls what network traffic is allowed into my machine. This week I am NOT setting the inbound security rules myself.
```

---

## Analysis Questions

**Analysis Question 1.** Why would an organization put every student machine in one small subnet instead of giving each machine a public address? *(Minimum 3 sentences.)*

```
An organization puts all the student machines into a subnet instead of giving each computer a public address because having all the machines on a subnet is segmenting the student computers from the rest of the local network. This gives a layer of protection for the rest of the network in case a student makes a mistake that affects their subnet floor, the issue will only be on that floor/subnet and not the rest of the network. Public IP address are limited, so having one public IP for all the student computers is helping to conserve public IPs.
```

**Analysis Question 2.** Segmentation means separating a network into parts that cannot freely reach each other. Give one concrete benefit of segmentation during a security incident. *(Minimum 3 sentences.)*

```
One benefit of segmentation during a security incident is that whatever the issue is, it will stay on that segmented subnet/floor. In order for the incident to spread onto other subnets/floors it has to cross another boundary (reach another subnet). Defenders can have a better chance at resolving the issue by denying access to the bad traffic so that it can't travel to another subnet/floor.
```

**Analysis Question 3.** A diagram and a live machine disagree about an address range. Which do you trust, what do you do next, and why? *(Minimum 2 sentences.)*

```
I trust the live machine. I'd run [ip addr] to confirm my private IP address and mask and run [ip route] to confirm my subnet address and mask. I'd use the subnet address and mask to manually determine the address range and compare that to the diagram. I'd also let someone know that the diagram needs to be updated once I confirm that the address range I calculated is correct.
```

---

## Submission Checklist

- [x] VNet name, address space, and subnet range recorded from the blueprint (Part A)

- [x] `ip addr` run and own private IP confirmed inside `10.60.6.0/26` (Part B, Step 1)

- [x] `ip route` run and default route behaviour explained (Part B, Step 2)

- [x] `blueprint-verified.png` captured from your own terminal, cropped, uploaded to `assets/screenshots/week-06/` (Part B, Step 3)

- [x] Private address / NAT / Bastion explained (Part C, Steps 1–2)

- [x] Per-student guard post identified — and explicitly not configured this week (Part C, Step 3)

- [x] All three Analysis Questions answered (minimum sentence counts met)

- [x] This file is committed to your portfolio repo at `week-06/labs/lab-04-reading-the-blueprints.md`

---

## GitHub Commit Subsection

1. Open **Week 6 → Lab 04: Reading the Blueprints** in the Lab Portal.
2. Fill in the worksheet fields and upload `blueprint-verified.png` to `assets/screenshots/week-06/`.
3. Click **Submit to GitHub** — the Portal commits to `week-06/labs/lab-04-reading-the-blueprints.md`.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
