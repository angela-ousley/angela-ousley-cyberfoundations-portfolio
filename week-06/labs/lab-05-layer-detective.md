# Week 6 Lab 05 — Layer Detective

**Student Name:** Angela Ousley

**Date Completed:** Aug 23, 2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 6  
**Submission Path:** `week-06/labs/lab-05-layer-detective.md`

---

## Overview

**This is a SHORT lab — 20 to 30 minutes — and it needs no VM.** No Cloud Heights session, no simulator, no screenshot. This is a thinking lab: you take the evidence you have already collected in Weeks 5 and 6 and sort it into layers.

This is an **independent** lab.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | This worksheet only — nothing to start, nothing to connect to |
| Prerequisite | Week 5 labs and Week 6 Labs 01–04 |
| Screenshot | None required |

---

## Part A — The Seven-Row Table

Fill in every row. For the last column, name one **real thing you personally saw** in Weeks 5–6 that belongs at that layer.

| # | Layer name | One-line job | Real thing from Weeks 5–6 |
| --- | --- | --- | --- |
| 7 | Application | Service you actually use | Logging into Azure Bastion |
| 6 | Presentation | Formatting data for receiving side | Encryption of unreadable packet in Wireshark |
| 5 | Session | Starts, maintains, and ends convo | Session in remote shell SSH 22 |
| 4 | Transport | Reliable delivery | TCP, UDP, ports viewed in Wireshark |
| 3 | Network | Sends messages between networks | traceroute bbc.co.uk |
| 2 | Data Link | Sends messages on local network | My virtual machine's MAC address |
| 1 | Physical | Raw signals- electricity and WiFi  | Plugging in my computer adapter |

---

## Part B — Case Files

For each case, name the layer where the problem lives, and name the evidence proving the layers **below** it were already working.

### Case File 1 — The Name That Went Nowhere

A hostname lookup fails, but pinging the machine's IP address directly succeeds.

Layer:

```
Layer 7, Application
```

Evidence that the layers below were working:

```
The hostname not resolving is a DNS (layer 7) protocol failing. Ping is a layer 3 (network) protocol. Ping getting a response from the website via IP address shows that at least layers 1 - 3 are working. 
```

### Case File 2 — Permission Denied

`ssh` to a host returns `Permission denied` after a password prompt.

Layer:

```
Layer 7, Application
```

Evidence that the layers below were working:

```
Getting to the password portion and getting 'permission denied' shows that the 6 layers below are working, you were able to make it to the application. Getting rejected at authentication is layer 7 failing, you weren't able to make it into the application
```

### Case File 3 — The Cable Story

A machine reports no link on its interface and has no address at all.

Layer:

```
Layer 1, Physical
```

Evidence and reasoning:

```
If you run the command [ip addr] and don't receive an address, that's a layer 1 issue because it's a reachability failure. The IP address could be misconfigured or a DHCP failure.
```

### Case File 4 — Ping Works, The Page Does Not

`ping` to a server succeeds, but `curl http://<that server>` returns nothing useful.

Layer:

```
Layer 7, Application
```

Evidence that the layers below were working:

```
Ping working shows that layer 3 is working fine and the server is reachable. Ping uses ICMP which is a layer 3 protocol, so layers 1 and 2 must be working. Since the server is reachable, I'm guessing layers 4-6 are working as well and layer 7 is the problem.
```

### Case File 5 — Wrong Neighbourhood

A machine has an address, but its default route points somewhere that cannot forward its traffic.

Layer:

```
Layer 3, Network
```

Evidence and reasoning:

```
Default gateways reside at the network (layer 3) level because that level deals with communicating across separate networks. Since the gateway can't forward traffic, the default address may be inactive or misconfigured.
```

---

## Part C — The Silent Gateway Case

In Lab 03 the Azure default gateway did not answer your ping. However, your VM had a valid default route configured, and your local communication with the Grid Beacon — the ping replies, the HTTP banner, and `TRACE ID: CF-NET-0604` — succeeded.

A failed gateway ping is one piece of evidence — not automatically proof of a gateway or network failure. But the evidence you weigh against it has to be the right kind of evidence.

The Grid Beacon at `10.60.6.4` sits on the same local subnet as your VM (`10.60.6.0/26`). Reaching it proves **local-subnet connectivity** — that traffic never crosses the default gateway, so beacon success alone cannot prove the gateway forwarded anything. Your `ip route` output proves a **default route is configured** — your VM knows where it intends to send non-local traffic — but it does not prove the gateway forwarded that traffic. The evidence that demonstrates the **default path is functioning** is successful communication with a destination outside `10.60.6.0/26`, such as the outbound internet access through NAT that you examined in Lab 04.

### Step 1 — Rule on the Case

Is the failed gateway ping enough evidence to declare a network-layer failure? Explain your answer using the other evidence you collected. In your response, distinguish between:

- evidence that proves **local-subnet connectivity**
- evidence that proves a **default route is configured**
- evidence that supports **successful off-subnet connectivity**

```
The failed gateway ping isn't enough evidence to declare a network-layer failure. Virtual machine platforms never answer a ping request. Getting a response from pinging the beacon proves that local-subnet connectivity is working because the beacon is inside of our network. The [ip route] command proves that the default gateway is setup. There is no evidence of successful off-subnet connectivity because we never sent anything to a separate network. In lab 04, we only explained how NAT works.
```

### Step 2 — Name the Correct Conclusion

For each of these four results, state what it actually proves: the Grid Beacon at `10.60.6.4` answering, the default route shown by `ip route`, a successful connection to a destination outside your local subnet, and the gateway's failed ping. Then state the rule you would give a junior colleague about the difference between an observation ("the gateway did not answer my ICMP probe") and a diagnosis ("the gateway is broken"):

```
I would let my junior colleague know that probes that go unanswered by a cloud platform is normal. A virtual machine's platform is set to not answer pings. The beacon answering proves that the local network is communicating properly. The default route shown by [ip route] shows that the outgoing traffic has a gateway to leave from. The gateway's failed ping is normal on a VM. We weren't asked to ping or use a command with an outside IP address, so we don't have proof of a successful connection to a destination outside of our local subnet. But if we did make a connection to a destination outside of the local subnet, it would prove that the default gateway is working because messages are making it to its destination outside of our subnet.
```

---

## Part D — Two Models, One Job

The OSI model has seven layers. The practical TCP/IP model most engineers speak day to day has four or five.

### Step 1 — Map Them

Briefly show how the seven OSI layers collapse into the practical model:

```
The application, presentation, and session (layers 5-7) layers of the OSI model go into the application (layer 4) layer of the TCP/IP model. The transport layer of the OSI model is the same for the TCP/IP model. The network layer (layer 3) of OSI is the internet layer (layer 2) for the TCP/IP model. The data link and physical layers (layers 1 & 2) of the OSI model make up the network access layer (layer 1) of the TCP/IP model.

```

### Step 2 — When Each Is Useful

Explain when the seven-layer vocabulary helps and when the practical model is the better tool:

```
The TCP/IP model is better to use for troubleshooting network issues. The OSI model is used more in documentation and interviews. TCP/IP is used in the technical aspect, while OSI is used more administratively. 
```

---

## Analysis Questions

**Analysis Question 1.** Explain the Ladder Rule using layer language. What does "test the near thing first" mean when the rungs are layers? *(Minimum 3 sentences.)*

```
Test the near thing first means testing your own network [ip addr], so that would include layers 1 and 2 of the OSI model. The second thing to test is your route [ip route], this includes layer 3. The third step would be to test a known working target inside the subnet [ping] which is layer 3 showing reachability. The fourth step is reaching outside of our network and using [dig] and the name of an IP address. This is DNS as that is layer 7. The fifth step is using [curl] with the IP address. The sixth step of the ladder is using [traceroute] to see how the packet makes it to the target.
```

**Analysis Question 2.** Why is "which layer is this?" a faster question than "what is broken?" when you are under pressure? *(Minimum 3 sentences.)*

```
"Which layer is this?" is a better question than "what is broken?" because if you can figure out what layer the issue is on, then you've narrowed down where the issue comes from. Once you know the layer, ti becomes a little easier to troubleshoot. You can start testing commands that target that layer and reviewing the responses to see if you get any error messages or insight as to what is broken.
```

**Analysis Question 3.** Pick one case file from Part B and describe the very next command you would run to confirm your ruling, and what result would change your mind. *(Minimum 2 sentences.)*

```
Case File 1 — The Name That Went Nowhere
Since pinging the machine's IP address directly succeeds, that lets me know that the website is reachable. The next command I would run would be [curl] and see if I can pull the content from the website. If curl pulls the content from the website, I'd say it's still a layer 7 DNS problem. If curl doesn't pull content, I'd say it's still a layer 7 problem, but the the added issue would be the web service being unavailable.
```

---

## Submission Checklist

- [x] All seven rows of the OSI table completed with a real Week 5–6 anchor each (Part A)

- [x] All five case files given a layer and supporting evidence (Part B)

- [x] Silent gateway case ruled on correctly (Part C)

- [x] OSI vs. practical TCP/IP model compared (Part D)

- [x] All three Analysis Questions answered (minimum sentence counts met)

- [x] No screenshot required for this lab

- [x] This file is committed to your portfolio repo at `week-06/labs/lab-05-layer-detective.md`

---

## GitHub Commit Subsection

1. Open **Week 6 → Lab 05: Layer Detective** in the Lab Portal.
2. Fill in the worksheet fields.
3. Click **Submit to GitHub** — the Portal commits to `week-06/labs/lab-05-layer-detective.md`.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
