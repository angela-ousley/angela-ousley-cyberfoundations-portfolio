# Week 2 Lab 01 — Cybersecurity Landscape & Digital Infrastructure Overview

**Student Name:** Angela Ousley

**Date Completed:** July 23, 2026

**Module:** 1 — Digital Infrastructure & CLI | **Week:** 2  
**Submission Path:** `week-02/labs/lab-01-hardware-os-software-diagram.md`

---

## Overview

In this lab, you build a working mental model of the system you'll be securing throughout this course: the hardware, operating system, and software layers that make up every computer, and where the cybersecurity field fits around them. This lab has two parts. Part A connects this week's material to the CyberFoundations City map. Part B has you build and explain a diagram of how a computer's hardware, OS, and software layers interact.

**No terminal or command line is required this week** — that starts in Week 3.

---

## Lab Environment

| Component | Details |
|---|---|
| Environment | Browser-based Lab Portal (Module 1 orientation) |
| Required Materials | CyberFoundations City map; a diagram tool of your choice (hand-drawn and photographed, or any digital tool) |

**Prerequisite:** Portfolio repo created from the CyberFoundations student template in Week 1. Fill out this worksheet here in the Lab Portal, then hit Submit to commit it directly to your repo at `week-02/labs/lab-01-hardware-os-software-diagram.md`.

---

## Part A — CyberFoundations City & the Cybersecurity Landscape

The CyberFoundations City map is your visual guide to the next 11 weeks. Each district represents a module of this course. This part connects this week's material to the map you were introduced to in Week 1.

### Step 1 — Open the Lab Portal Orientation Module

From your Student Dashboard, open the **Module 1 orientation** module.

### Step 2 — Complete the Orientation Walkthrough

Work through the orientation content. It covers the same hardware/OS/software material as this week's lessons from a different angle — use it to check your understanding, not to replace the lessons.

### Step 3 — Locate This Week's District on the City Map

Open the CyberFoundations City map (introduced in Week 1, Lesson 6). Identify which district corresponds to Module 1 — Digital Infrastructure & CLI.

**District name:**

```
Foundry District
```

**Why this district fits this week's topics (1–2 sentences):**

```
The Foundry district fits this weeks topics because this district is based on cybersecurity foundations. Learning about the cybersecurity landscape, what's inside a computer, and operating systems is foundational knowledge that allows us to understand the digital infrastructure we are protecting.
```

---

## Part B — Hardware, OS, and Software Diagram

A computer is a stack of layers: physical hardware at the bottom, an operating system managing that hardware in the middle, and the software you actually use on top. This part has you draw that stack and explain it in your own words.

### Step 1 — Identify the Layers

Before drawing anything, name one example of what lives at each layer.

**Hardware layer — one example component (e.g., CPU, RAM, or storage):**

```
RAM, the computer's memory. It is short-term and volatile. The memory is erased once the computer is shutdown.
```

**Operating system layer — name an OS (e.g., Windows, Linux, or macOS):**

```
Windows is an operating system created by Microsoft that is proprietary (the source code is confidential).
```

**Software layer — one example application (e.g., a web browser or word processor):**

```
Microsoft Teams is a software used on Windows were instant messages can be sent to users within the same company or group.
```

### Step 2 — Sketch Your Diagram

Sketch a simple diagram (hand-drawn and photographed, or built in any digital tool) showing how the hardware, OS, and software layers stack and interact. Arrows or labels showing "what talks to what" matter more than visual polish. If you'd like a free browser-based option instead of hand-drawing, try [draw.io](https://www.drawio.com/) — no account required to get started.

### Step 3 — Upload and Embed Your Diagram

This step happens directly on GitHub, not through this worksheet — there's no upload field here, since screenshots and diagrams are added through GitHub's own upload UI, the same way as every other week.

1. Go to your portfolio repository on GitHub.com and navigate to `assets/screenshots/week-02/`.
2. Click **Add file → Upload files**, then drag in your diagram image (name it something descriptive, like `hardware-os-software-diagram.png` — no spaces, lowercase, no timestamps).
3. Commit the upload.
4. Click on the uploaded image to open it, then click the **Raw** button. Copy the URL from your browser's address bar.
5. After you submit this worksheet, it will be committed to your repo. Go back to GitHub, open the committed file, click the pencil (edit) icon, and paste your raw URL into the embed line below:

![Hardware OS Software Diagram](https://raw.githubusercontent.com/angela-ousley/angela-ousley-cyberfoundations-portfolio/refs/heads/main/assets/screenshots/week-02/hardware_os_software%20diagram_angela_ousley_1.png)

**My Diagram** (added directly on GitHub after you submit):

### Step 4 — Explain Your Diagram

In your own words — not a copied definition — explain how the three layers interact. Reference your own diagram directly.

**Explanation (minimum 3 sentences):**

```
When you turn on a computer, the firmware (BIOS or UEFI) gets the hardware ready to operate. The BIOS pulls the Operating System (OS) from the storage (SSD or HDD) and puts it into memory (RAM). The OS appears on the computer monitor and the user can login or begin to interact with the interface. Once a user opens a software application, the app asks the OS for CPU time. The OS assigns the app a piece of RAM. The CPU executes instructions from the app. The interface shows the results on the computer monitor.
```

---

## Analysis Questions

Answer each question in your own words. These questions connect what you did in Parts A and B to the bigger picture of this course.

### Analysis Question 1

If the operating system crashed on the computer you diagrammed, which layer(s) would stop working, and which (if any) would keep working? Explain your reasoning.

```
If the operating system crashed on my computer that I diagramed, the software layer would stop working for sure because that relies on the OS layer. The OS layer wouldn't work either because it crashed. The hardware layer would still function and try to reload the OS once the system is restarted.
```

### Analysis Question 2

Pick one piece of software you use daily. Trace it down through the OS to the hardware it ultimately depends on. What would happen to that software if the hardware layer failed?

```
I use Microsoft Teams daily while at work. The Teams app traces down to the Windows operating system and into the storage disk of the hardware. If the hardware layer failed, the Teams app, or any other software, would no longer be accessible. With the hardware failing, the apps can't be accessed from the storage disk or loaded into RAM. The OS wouldn't be able to talk to the hardware and vice versa, so the OS wouldn't function and therefore the software layer wouldn't function either.
```

### Analysis Question 3

Explain, in your own words, why a cybersecurity professional needs to understand all three layers — hardware, OS, and software — rather than just the software layer where most visible attacks (like phishing emails) happen.

```
A cybersecurity professional needs to understand all three layers because attacks can happen at all three layers. A rootkit can be installed on the software, OS, or hardware layer. If a computer starts to have issues, you need to be able to pinpoint where the issue is coming from; what layer, what piece of hardware, the OS, or which software application is infected.
```

---

## Lab Report Questions

Answer each question in complete sentences.

**1. What is the cybersecurity landscape, and why does it matter to someone starting this course?**

```
The cybersecurity landscape includes all the assets that need to be defended and protected from possible attacks. It includes the attackers, defenders, infrastructure that's being protected, and everyday users. This matters to someone starting this course because if they are planning on being a defender, they'll need to know where they fit into this landscape and how the various parts interact with each other because they'll be working within that landscape daily.
```

**2. Which CyberFoundations City district did you identify in Part A, and how does its theme connect to the hardware/OS/software material in Part B?**

```
I identified the Foundry district in Part A. Its theme connects to the hardware/OS/software material in Part B because it is teaching us about the digital infrastructure which is part of the foundations of information technology. The hardware/OS/software layers are foundational knowledge when it comes to IT and are also considered attack surfaces in which a cybersecurity professional will need to protect.
```

**3. Of the three layers (hardware, OS, software), which one do you think is hardest to secure, and why?**

```
I think the hardware layer is the hardest to secure because malware at the lower level is a bit harder to detect with a typical malware or antivirus scan. A rootkit installed on the BIOS can go undetected and is very hard to remove even when it is detected. The BIOS may need to be reset to attempt to remove the malware, or replaced completely to make sure the rootkit is gone.
```

---

## Submission Checklist

- [x] Lab Portal Module 1 orientation completed

- [x] District identified and explained

- [x] Hardware, OS, and software layer examples listed

- [x] Diagram uploaded to `assets/screenshots/week-02/` via GitHub and embedded directly in the committed file

- [x] Diagram explanation written in your own words (minimum 3 sentences)

- [x] All three Analysis Questions answered (minimum sentence counts met)

- [x] All three Lab Report Questions answered in complete sentences

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
