# Week 6 Stretch Lab — Night Watch (Optional)

**Student Name:** Angela Ousley

**Date Completed:** September

**Module:** 2 — Networking & Cloud Foundations | **Week:** 6  
**Submission Path:** `week-06/labs/lab-06-stretch-night-watch.md`

---

## Overview

**This lab is optional.** Skipping it costs you nothing. Your Week 6 submission is complete and full-credit with Labs 01–05 alone, and choosing not to do this does not make your week's work lesser in any way. It is here for students who want to connect Cloud Heights to the network they actually live on. Expect about 30 minutes if you take it on.

**Built-in tools only.** Use commands that already ship with your own computer. **Do not install any software** for this lab.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Your own personal computer — not Cloud Heights |
| Tools | Built-in only: `ipconfig` / `Get-NetIPConfiguration` (Windows) or `ifconfig` / `ip addr` / `netstat -rn` (macOS/Linux), plus `ping` |
| Installs | None. If it needs installing, it is out of scope. |
| Screenshot | Required **only if you submit**: `stretch-real-network.png`, redacted |

> ### 🔒 Redaction Rule
> If you submit this lab, you must redact before uploading. Never publish your home network identity.

---

## Part A — Your Own Address

### Step 1 — Read Your Machine's Network Configuration

Use the built-in command for your operating system to show your address, subnet mask/prefix, and default gateway.

Command you ran:

```
[ifconfig en0] to see my ip address

It's easier for me to go to my Network settings to see my ip address, subnet mask, and default gateway all at once.
```

Your private IP and prefix/mask (this is a private address — safe to record):

```
10.0.0.181/24
```

Your default gateway (private address — safe to record):

```
10.0.0.1
```

**Do not record your public IP address anywhere in this file.**

### Step 2 — Compare to Cloud Heights

Compare your home addressing to your Cloud Heights addressing — address range, prefix size, and how many machines each network could hold:

```
My Cloud Heights address is 10.60.6.35/26 and my home address is 10.0.0.181/24.

The /26 mask means that the first 3 octets (10.60.6) are for the network and a quarter of the 4th octet (2 bits) are also for the network. The 2 network bits in the last octet leave 6 host bits to work with. 2^6 = 64 so that means there are 64 addresses available on Cloud Heights ranging from 0 to 63. So the Cloud Heights network can hold 62 machines (64 total addresses - 1 network address - 1 broadcast address = 62 available). The address range would be 10.60.6.1 to 10.60.6.62.

The /24 mask means that the first 3 octets (10.0.0) are for the network and the last octet is for the hosts. 8 host bits available calculates to 2^8 = 256 addresses available on my home network. My network can hold 254 machines (256 total addresses - 1 network address - 1 broadcast address = 254 usable addresses). The range is from 10.0.0.1 - 10.0.0.254.
```

---

## Part B — Two Gateways, Two Behaviours

### Step 1 — Ping Your Home Gateway

Ping your own default gateway.

Output:

```
[ping 10.0.0.1 -c4]

PING 10.0.0.1 (10.0.0.1): 56 data bytes
64 bytes from 10.0.0.1: icmp_seq=0 ttl=64 time=4.197 ms
64 bytes from 10.0.0.1: icmp_seq=1 ttl=64 time=3.794 ms
64 bytes from 10.0.0.1: icmp_seq=2 ttl=64 time=4.984 ms
64 bytes from 10.0.0.1: icmp_seq=3 ttl=64 time=4.427 ms

--- 10.0.0.1 ping statistics ---
4 packets transmitted, 4 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 3.794/4.351/4.984/0.430 ms
```

### Step 2 — Compare to Azure's Silent Gateway

Your home router almost certainly answered. The Azure gateway in Lab 03 did not.

Explain why both behaviours are normal, and what that teaches you about assuming a non-answer means failure:

```
Both behaviors are normal. Azure Bastion never answers to a [ping] because it's configured not to, so that's expected. Not getting a response from a ping doesn't mean there is a failure. The absence of an answer is not evidence of failure because ping only tells you if the message returns from the address you pinged, not if the machine is up and running.
```

---

## Part C — Redaction (Required Only If You Submit)

**Required filename:** `stretch-real-network.png`

Redact **before** uploading. Redaction targets:

- your **public IP address**
- your computer's **hostname**
- your **shell username**
- any **ISP-identifying names** (router model strings, provider names, SSIDs)

**Method:** crop the image, or cover the text with **solid opaque boxes**. **Do not use blur or pixelation** — both can be reversed.

![Home network configuration — redacted](https://raw.githubusercontent.com/angela-ousley/angela-ousley-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-06/stretch-real-network.png)

List what you redacted and the method you used:

```
I redacted my computer's name and my shell username. I used tools to get to 'annotate' and used the rectangle to cover my private info. Then I took a screenshot of the screenshot to ensure my redactions can't be reversed.
```

---

## Analysis Questions

**Analysis Question 1.** What is genuinely different between the network you sit on at home and the one Cloud Heights sits on, and what is essentially the same? *(Minimum 3 sentences.)*

```
What's different between my home network and the Cloud Heights network is that my network has physical devices like cables, a Wi-Fi router, and a NIC. A VM network like Cloud Heights uses a virtual devices like a vSwitch and a vNIC. Both types of networks use subnetting, NAT, and DHCP.
```

**Analysis Question 2.** Why is publishing your public IP, hostname, and username together riskier than publishing any one of them alone? *(Minimum 3 sentences.)*

```
Publishing multiple types of private information about yourself or your network is riskier than posting just one thing because the more information an attacker has about you and your system, the easier it is for them to breach it. Your public IP address gives out your geographical location. Although it cannot reveal your physical home address on its own, a motivated attacker can cross-reference your IP with other public information (like social media posts) to try and piece together your real identity. Publishing your hostname on the internet makes it easier for hackers to target your accounts, especially if you reuse that same username across multiple websites.
```

---

## Submission Checklist (Only If You Choose to Submit)

- [x] Home address, mask, and gateway recorded — **public IP not recorded**

- [x] Home network compared to Cloud Heights (Part A, Step 2)

- [x] Home gateway pinged and compared to Azure's silent gateway (Part B)

- [x] `stretch-real-network.png` redacted with crop or solid boxes (no blur), uploaded to `assets/screenshots/week-06/`

- [x] Redaction list recorded (Part C)

- [x] Both Analysis Questions answered

- [x] This file is committed to your portfolio repo at `week-06/labs/lab-06-stretch-night-watch.md`

---

## GitHub Commit Subsection

1. Open **Week 6 → Stretch Lab: Night Watch** in the Lab Portal.
2. Fill in the worksheet fields and upload your redacted screenshot to `assets/screenshots/week-06/`.
3. Click **Submit to GitHub** — the Portal commits to `week-06/labs/lab-06-stretch-night-watch.md`.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
