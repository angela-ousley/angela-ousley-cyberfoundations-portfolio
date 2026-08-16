# Week 5 Lab 03 — Reading the Grid's Mail (Packet Inspector)

**Student Name:** Angela Ousley

**Date Completed:** August 16, 2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 5  
**Submission Path:** `week-05/labs/lab-03-reading-the-grids-mail.md`

---

## Overview

Lesson 4 showed you the three panes of a packet analyzer — the packet list, the detail pane, and the filter bar — and told you that professionals live in this tool. This lab is where you stop watching and start driving. You'll open a recorded minute of traffic on The Grid and read it packet by packet: a name lookup (Part B), a handshake and the message that followed it (Part C), and one conversation that refuses to be read at all.

**The capture is the same for every student.** It's a recording, not a live network — fifteen packets, captured once, replayed identically for everyone. That means your numbers should match your classmates' exactly. If yours don't, you've filtered something out; clear the filter and look again.

**Nothing here can break anything.** You are reading a recording. There is no traffic to disturb and nothing to configure.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | CyberFoundations **Packet Inspector** — browser-based, inside the Lab Portal. Nothing to install. You will never install Wireshark for this course |
| Where to find it | Lab Portal main navigation, alongside the **CLI Simulator** |
| Capture | "The Grid — Workstation Capture." Canned and identical for every student |
| Prerequisite | Week 5, Lesson 3 (ports, the handshake) and Lesson 4 (the three panes) completed. Lab 01 recommended first — Part B builds directly on it |
| Filters available | `dns` · `icmp` · `tcp` · `http` · `ip.addr == x.x.x.x` · `tcp.port == N` |
| Time | Plan for 30–40 minutes, including this worksheet |

**Before you start:** log into the Lab Portal, open **Week 5 → Packet Inspector**, and load the capture named **"The Grid — Workstation Capture."** You should see a list of packets across the top, an empty detail pane below it, and a filter bar above everything. Keep this worksheet open in a second browser tab so you can record answers as you go.

**Get your screenshot tool ready now.** **Windows:** `Win + Shift + S` · **Mac:** `Cmd + Shift + 4`, then drag to capture. You'll need it twice in this lab, and both screenshots are required.

**A note on filters:** a filter never deletes packets. It hides the ones that don't match so you can concentrate. Clearing the filter box brings everything back. This matters in Part A — count *before* you filter anything.

---

## Part A — Orientation

### Step 1 — Count the Whole Capture

With the filter bar **empty**, look at the packet list and count how many packets the capture contains. Every packet has a number in the first column, so the last row tells you the answer without counting by hand.

The total number of packets in the capture:

```
15
```

### Step 2 — Inventory the Protocols

Scan the **Protocol** column from top to bottom and write down every distinct protocol name you see. There are five. You met all five in Lessons 2, 3 and 4 — this is the first time you're seeing them as labelled traffic rather than as ideas.

The protocols that appear in this capture:

```
DNS, ICMP, TCP, TLS, and HTTP.
```

### Step 3 — Read the Columns

Four columns do most of the work in a packet analyzer. In your own words — not the Lesson 4 wording — write one plain sentence for each: what does it tell you, and what question does it answer?

What the Source column tells you:

```
The source column tells you the IP address of where the packet came from.
```

What the Destination column tells you:

```
The destination column tells you the IP address of where the packet was sent to.
```

What the Protocol column tells you:

```
The protocol column tells you which rule was used to send/receive the data.
```

What the Info column tells you:

```
The info column tells you a few details about the packet.
```

### Step 4 — Spot Your Own Machine

One address appears more often than any other, and it's the one sending most of the requests. That's the workstation the capture was taken from — the same address you recorded in Lab 01.

The workstation's IP address, and how you worked out it was the workstation:

```
10.20.5.42 is the workstation's IP address. I figured this was it because it appears the most in the source column and it was told to us in Lesson 1 that this is our workstation.
```

---

## Part B — Follow a DNS Lookup

In Lab 01 you ran `dig foundry-archive.grid.local` and read a tidy answer: a name went in, an IP address came out. You saw the *result*. Now you're going to see the actual conversation that produced it — the question travelling across the wire and the answer coming back.

### Step 1 — Filter for DNS

Type `dns` into the filter bar and apply it. The packet list shrinks to the name-lookup traffic only.

The packet numbers that remain after the `dns` filter:

```
1 and 2
```

### Step 2 — Read the Question

Click the first of your two DNS packets and read its Info column and detail pane. This is the query — your workstation asking the Directory Board a question.

The packet number of the query, and the name being asked about:

```
Packet number 1 and the name being asked about is foundry-archive.grid.local
```

The source and destination addresses of the query — who asked, and who was asked:

```
The source address is 10.20.5.42 and the destination address is 10.20.5.10
```

### Step 3 — Read the Answer

Now click the second DNS packet. This is the response coming back the other way.

The packet number of the response, and the IP address it returned:

```
Packet number 2 and the IP address returned is 10.20.5.20
```

### Step 4 — Find the Door

DNS doesn't just travel to an address — it travels to a specific **port** on that address, the way Lesson 3 described a numbered door on a building. Open the detail pane on either DNS packet and find the port number the lookup used.

The port number DNS used here:

```
53
```

### Step 5 — Tie It Back to Lab 01

This is the same lookup you ran with `dig` in Lab 01 — same name, same answer, same DNS server. The difference is the altitude you're viewing it from. `dig` handed you the conclusion; the Packet Inspector shows you the two messages that produced it.

In two or three sentences: what did the packet view show you that `dig` did not?

```
The packet shows the source port and source address, so it showed our address and port for when we sent the DNS request. It also showed that UDP was used to send the packet and the Time to Live was 64. The dig command didn't show these details.
```

### Step 6 — Capture Screenshot 1 (REQUIRED)

With the `dns` filter still applied and one of the two DNS packets selected, take a screenshot showing the filter bar, the filtered packet list, and the detail pane. Name it exactly **`packet-dns-query.png`**. Upload instructions are in the GitHub Commit section.

---

## Part C — Doors and the Handshake

Lesson 3 told the TCP handshake as a knock at a door: **SYN** ("knock"), **SYN-ACK** ("who's there — come in"), **ACK** ("thanks"). You're about to watch one happen.

### Step 1 — Filter for Port 443

Clear the `dns` filter and enter `tcp.port == 443` instead. Port 443 is HTTPS — one of the well-known doors from Lesson 3.

The packet numbers that remain after the `tcp.port == 443` filter:

```
7,8,9, and 10
```

### Step 2 — Identify the Three-Step Handshake

Three of those packets are the handshake itself. Find each one by reading the flags in the Info column, and record its number, its direction (which address sent it to which), and which step of the handshake it is.

The SYN — packet number and direction:

```
The SYN packet is number 7. It came from 10.20.5.42 and was sent to 10.20.5.20
```

The SYN-ACK — packet number and direction:

```
The SYN-ACK packet number is 8. It came from 10.20.5.20 and was sent to 10.20.5.42
```

The ACK — packet number and direction:

```
The ACK packet number is 9. It came from 10.20.5.42 and was sent to 10.20.5.20
```

### Step 3 — Notice the Two Port Numbers

Look at the Info column on the SYN packet. Two port numbers appear: a high, odd-looking one on your workstation's side, and 443 on the server's side. Only one of them is a "well-known door" — the other is a temporary one your machine picked for this conversation.

The two port numbers, and which belongs to the server:

```
The two port numbers are 51514 and 443. The server port is 443.
```

### Step 4 — Switch to the Plain Conversation

Clear the filter and enter `http` instead. This is a different conversation to a different machine on The Grid — the notice board — on port 80.

The packet numbers that remain after the `http` filter:

```
14 and 15
```

### Step 5 — Open the Request and Read It

Click **packet 14** and expand its detail pane all the way. Unlike everything you've read so far, this packet's contents are ordinary text — you can simply read them, the way you'd read a note left on a desk.

The request line at the top of packet 14 (the method, the page, and the version):

```
Request Method: GET
Request URI: /shift-notice.html
Request Version: HTTP/1.1
```

The Host line — which machine the request was addressed to:

```
Host: grid-notice.grid.local
```

Every other readable line in packet 14's detail pane:

```
GET /shift-notice.html HTTP/1.1
Host: grid-notice.grid.local
User-Agent: GridBrowser/2.4
X-Staff-Code: FOUNDRY-2026-STOREROOM
```

### Step 6 — Notice What You Just Read

One of those lines is not like the others. It carries a value that looks like an internal code rather than a technical setting.

The name of that header and the exact value it carries:

```
X-Staff-Code: FOUNDRY-2026-STOREROOM
```

In one or two sentences: if you were sitting on this network with a packet analyzer open, what would you now know that you were never meant to know?

```
I would now know the staff code to the foundry storeroom.
```

### Step 7 — Capture Screenshot 2 (REQUIRED)

With the `http` filter applied and **packet 14** selected and expanded so the readable lines are visible, take a screenshot. Name it exactly **`packet-http-plaintext.png`**.

### Step 8 — Now Try to Read the Other One

Clear the filter and click **packet 10** — the TLS Client Hello from the port 443 conversation you examined in Steps 1–3. Expand its detail pane and try to read it the same way you just read packet 14.

What packet 10's detail pane shows you, described in your own words:

```
It looks like a hexadecimal data.
```

Compare it to packet 14 — what can you still tell about packet 10 from the packet list, even though you can't read its contents?

```
I can tell packet 10 is encrypted and that's why we can't read its contents. Its destination port is 443 (https).
```

**Don't chase this yet.** You've just found something real, and the explanation is a later part of this course. For now, sit with the observation: two conversations, one legible and one not. Analysis Question 4 asks what you make of it.

---

## Analysis Questions

**Analysis Question 1.** In Part A you counted the packets before applying any filter. Explain why a filter is a *view* rather than a deletion, and describe a situation where forgetting that could lead an analyst to a wrong conclusion. *(Minimum 3 sentences.)*

```
A filter is a view and not a deletion because you're focusing on the records that you want to see and hiding the records that you don't want to see. You can always change the view back by removing the filter to show all the records. Forgetting that a filter is on could lead an analyst to a wrong conclusion because they might not be able to find a record that is currently hidden. For example, if I apply a filter to only show the packets with ICMP protocol and I forget to remove that while I'm looking for packets with TCP protocol I will think that there aren't any packets using TCP protocol because they'll be hidden due to the ICMP filter.
```

**Analysis Question 2.** You have now seen the same DNS lookup twice — once as `dig` output in Lab 01, once as two packets here. Describe what each view is good for, and name one investigation where you would specifically want the packet view. *(Minimum 3 sentences.)*

```
Using [dig] on the command line is good for finding out if the name of the server resolves into an IP address. It also shows if the name doesn't resolve into an IP address (NXDOMAIN) and the IP address of the server that answered the DNS request. The packet view shows the same details, but shows the actual transmissions that produced the [dig] response. If you want more details about the DNS request/response, the packet view should be used. The packet can be used to investigate the source port and source address of when the DNS request was made and the IP version used.
```

**Analysis Question 3.** The handshake took three packets and about three milliseconds before a single byte of real content moved. Using the knock-at-the-door analogy from Lesson 3, explain what those three packets accomplish and why a connection would be less reliable without them. *(Minimum 3 sentences.)*

```
The SYN packet is the "knock". The SYN-ACK packet is the "who's there? Come in". The ACK packet is the "thank you". These three packets establish a connection before any data is sent between the two machines. This is TCP or the 3-way handshake. A connection like UDP is less reliable because it doesn't use the handshake, it just sends data without confirming a connection first.
```

**Analysis Question 4.** You read packet 14 word for word, and packet 10 not at all — same capture, same network, same workstation. What do you think explains the difference between them? And why does it matter that the one you *could* read contained a staff code? You are not expected to know the mechanism yet; give us your best reasoning from what you observed. *(Minimum 4 sentences.)*

```
The difference between the two is the protocol used. Packet 14 was sent using HTTP, which is unencrypted so it shows in readable plain text. Packet 10 was sent using TLS and that's an encrypted protocol so the text is scrambled. Being able to read a contained staff code matters because someone could pickup this packet over the network, read its contents, and gain access to the storeroom.
```

---

## Submission Checklist

- [x] Total packet count recorded with the filter bar empty (Part A, Step 1)

- [x] All five protocols listed (Part A, Step 2)

- [x] Source, Destination, Protocol and Info columns each explained in your own words (Part A, Step 3)

- [x] Workstation address identified with reasoning (Part A, Step 4)

- [x] `dns` filter applied; query and response packets identified by number (Part B, Steps 1–3)

- [x] Hostname queried, IP address returned, and port number recorded (Part B, Steps 2–4)

- [x] Lab 01 comparison written (Part B, Step 5)

- [x] **REQUIRED:** `packet-dns-query.png` uploaded to `assets/screenshots/week-05/` and its filename recorded (Part B, Step 6)

- [x] `tcp.port == 443` filter applied; SYN, SYN-ACK and ACK identified by number *and* direction (Part C, Steps 1–2)

- [x] `http` filter applied; the remaining packet numbers recorded (Part C, Step 4)

- [x] Both port numbers recorded, server's port identified (Part C, Step 3)

- [x] Packet 14's readable lines recorded, including the staff-code header and its exact value (Part C, Steps 5–6)

- [x] **REQUIRED:** `packet-http-plaintext.png` uploaded to `assets/screenshots/week-05/` and its filename recorded (Part C, Step 7)

- [x] Packet 10 opened and its contrast with packet 14 described (Part C, Step 8)

- [x] All four Analysis Questions answered (minimum sentence counts met)

- [x] This file is committed to your portfolio repo at `week-05/labs/lab-03-reading-the-grids-mail.md`

---

## GitHub Commit Subsection

This lab's written answers are submitted through the **CyberFoundations Lab Portal**, the same way as Week 4.

1. Go to the CyberFoundations Lab Portal and sign in.
2. Open **Week 5 → Lab 03: Reading the Grid's Mail**.
3. Fill in the worksheet fields — they match the steps and questions in this file.
4. Connect your GitHub account if you haven't already (one-time setup), and select your portfolio repo.
5. Click **Submit to GitHub**. The Portal commits the completed file to `week-05/labs/lab-03-reading-the-grids-mail.md` for you.

**📸 REQUIRED — both screenshots.** This lab has two, and both are graded:

| Screenshot | From | Filename |
|---|---|---|
| Filtered DNS view with a query packet selected | Part B, Step 6 | `packet-dns-query.png` |
| Packet 14 expanded, readable lines visible | Part C, Step 7 | `packet-http-plaintext.png` |

1. Go to your portfolio repository on GitHub.com and navigate to `assets/screenshots/week-05/` (create the folder if this is your first Week 5 screenshot).
2. Click **Add file → Upload files**, drag both images in, named exactly as above (lowercase, hyphens, no spaces).
3. Scroll down and click **Commit changes**.
4. Click each uploaded image's filename to open it and confirm the packet detail is readable at full size.
5. Record both filenames below so your grader knows to look for them.

The filename of your DNS screenshot:

```
packet-dns-query.png
```

The filename of your packet 14 screenshot:

```
packet-http-plaintext.png
```

Both screenshots live in `assets/screenshots/week-05/` in your repository. They do not need to be linked inside this worksheet.

**Commit message tip:** name the work, not the file type — *"Add Week 5 Lab 03 packet analysis evidence"* reads far better to an employer browsing your repo than "update."

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
