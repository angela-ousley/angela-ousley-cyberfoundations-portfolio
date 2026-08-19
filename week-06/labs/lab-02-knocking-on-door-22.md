# Week 6 Lab 02 — Knocking on Door 22

**Student Name:** Angela Ousley

**Date Completed:** Aug 19, 2026

**Module:** 2 — Networking & Cloud Foundations | **Week:** 6  
**Submission Path:** `week-06/labs/lab-02-knocking-on-door-22.md`

---

> ### 🔒 Cloud Heights Security Rule
> Your Bastion link and Cloud Heights password are **private access credentials**. Never paste either into a worksheet, screenshot, GitHub repository, Circle post, or chat message. When taking screenshots, crop out the browser address bar and all login information.

---

## Overview

Week 5 told you SSH is how administrators reach a machine over the network, and that it knocks on **port 22**. This week you knock yourself. You are already inside Cloud Heights through Bastion — now you will open a second, nested SSH session from your machine *to itself* and watch every step of what SSH does before it lets you in.

Starts **guided**, finishes **independent**. Expect 30–40 minutes.

**This lab uses password authentication only.** SSH keys are Week 8. Do not go looking for them yet.

---

## Lab Environment / Pre-Lab Check

| Component | Details |
|---|---|
| Environment | Cloud Heights — live Ubuntu 22.04 VM, reached through Azure Bastion |
| Username | `analyst` |
| Password | Provided separately. Never typed into this worksheet. |
| Commands used | `ssh`, `whoami`, `hostname`, `pwd`, `exit` |
| Prerequisite | Week 6 Lab 01 completed |

**Before you start:** open **My Lab Environment**, start your VM if needed, wait for **Running**, then open Cloud Heights.

---

## Part A — Two Ways Into the Same Room

### Step 1 — Name the Path You Already Used

You reached Cloud Heights through a browser session. Something else handled the network hop for you.

Describe, in your own words, what the Bastion/browser path did on your behalf:

```
The Bastion acted as a client on my behalf. It initiated the connection to the remote machine.
```

### Step 2 — Predict the Manual Path

You are about to type an SSH command by hand. Before you run it, write what you expect to happen and what you expect to be asked for:

```
I expect to get asked about a fingerprint and then asked for my password.
```

---

## Part B — Knocking

### Step 1 — Run the SSH Command

In your Cloud Heights terminal, run:
```
ssh analyst@localhost
```

**Stop before typing anything else.**

### Step 2 — Read the First-Connection Prompt

The first time SSH connects to a host it has never seen, it shows you the host's **fingerprint** and asks whether you want to continue connecting. This is not an error. It is SSH telling you: *I have no record of this machine yet — do you recognise it?*

Paste the prompt you saw (fingerprint line included — a fingerprint is not a credential):

```
analyst@cf-student-16:~$ ssh analyst@localhost
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ED25519 key fingerprint is SHA256:y4WygUmvp8CJqf1qgZZJwPyYWGb7pmYGi9OsJ238bm8.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? 
```

Explain why you were willing to answer `yes` here — what makes this an expected first connection rather than a suspicious one:

```
I was willing to answer 'yes' because this was my first time connecting to my machine via SSH and this is an expected result of trying to connect for the first time. SSH doesn't recognize my machine since this is my first time logging in.
```

### Step 3 — Enter Your Password

Type `yes`, then enter your password when prompted.

**Linux does not echo password input.** No characters, no dots, no asterisks appear as you type. The terminal looks frozen. It is not — type the password and press Enter.

What did the screen show while you typed:

```
It showed "analyst@localhost's password: ", but it didn't show my password as a typed it in.
```

### Step 4 — Prove You Are in the Nested Session

Inside the new session run each of these and record the output:
```
whoami
```

```
analyst
```

```
hostname
```

```
cf-student-16
```

```
pwd
```

```
/home/analyst
```

### Step 5 — Notice the Prompt

Compare the prompt now to the prompt before you ran `ssh`. Describe anything that changed and anything that looks identical, and explain why it looks that way given where you connected to:

```
The prompt looks the same as before I ran 'ssh'. It looks the same because it is the same. I'm was already inside Cloud Heights through Bastion. Using SSH to just allowed me to login to my remote machine again, that's why both prompts show 'analyst@cf-student-16:~$'.

```

### Step 6 — Capture Your Evidence

Screenshot the terminal showing the first-connection prompt and the successful session.

**Required filename:** `ssh-first-connection.png`

**Crop rules.** No Bastion URL, no address bar, no password field, no login screen. The fingerprint text is fine.

![SSH first connection and nested session 1](https://raw.githubusercontent.com/angela-ousley/angela-ousley-cyberfoundations-portfolio/2f51121dffc154e323f271c0514eeddb3c9a4058/assets/screenshots/week-06/ssh-first-connection-1.png)
![SSH first connection and nested session 2](https://raw.githubusercontent.com/angela-ousley/angela-ousley-cyberfoundations-portfolio/2f51121dffc154e323f271c0514eeddb3c9a4058/assets/screenshots/week-06/ssh-first-connection-2.png)
![SSH first connection and nested session 3](https://raw.githubusercontent.com/angela-ousley/angela-ousley-cyberfoundations-portfolio/2f51121dffc154e323f271c0514eeddb3c9a4058/assets/screenshots/week-06/ssh-first-connection-3.png)

### Step 7 — Leave

Run:
```
exit
```
What did the prompt look like after exiting, and how do you know you are back in the original session:

```
analyst@cf-student-16:~$ exit
logout
Connection to localhost closed.
analyst@cf-student-16:~$ 

I knew I was back in the original session because it reads 'connection to localhost closed' and the prompt reads 'analyst@cf-student-16:~$' which showed prior to logging in with SSH localhost.
```

---

## Part C — The Deliberate Failure (Independent)

### Step 1 — Knock With the Wrong Name

Run an SSH command to `localhost` using a username that does not exist on this machine — for example `ssh notauser@localhost`. Enter anything at the password prompt.

Command you ran:

```
ssh notauser@localhost
```

Output:

```
analyst@cf-student-16:~$ ssh notauser@localhost
notauser@localhost's password: 
Permission denied, please try again.
notauser@localhost's password: 
```

### Step 2 — Read the Failure Correctly

`Permission denied` is a **failure of authentication**, not a failure of the network.

Explain what the network and SSH already had to do successfully in order for you to be told "permission denied" at all:

```
The network and SSH had to reach the machine first in order for me to be told "permission denied". It had to go through routing and getting to port 22. You have to reach SSH port 22 first then authenticate to gain access.
```

---

## Analysis Questions

**Analysis Question 1.** Distinguish *reach* from *authentication*. Which one had already succeeded when you saw a password prompt, and how do you know? *(Minimum 3 sentences.)*

```
Reach is just the first step of SSH. Making the connection to port 22 is the reach. You wouldn't be asked for a password without making the connection first. Being asked for a password is the second step, so that's how you know the first step (connection) worked.
```

**Analysis Question 2.** You accepted a host fingerprint today because you knew you had just connected to your own machine. Describe a situation where accepting a fingerprint without thinking would be a real problem. *(Minimum 3 sentences.)*

```
A situation where accepting a fingerprint without thinking would start a problem would be during a man-in-the-middle attack. If a fingerprint is sent to you while you are trying to connect to SSH, but you don't check the key fingerprint to make sure it's valid, it could be an attacker sending you their key instead. After you accept (say yes), they now have access to your machine because typing in 'yes' tells the computer to trust that fingerprint key.
```

**Analysis Question 3.** What changed and what stayed the same when you moved from the outer session into the nested SSH session, and why? *(Minimum 2 sentences.)*

```
Nothing changed when I moved from the outer session into the nested SSH session. Everything stayed the same because I was logging into the same machine for a second time. That's why the prompt looks exactly the same in the first login session and nested login session.
```

**Analysis Question 4.** A colleague says "SSH is broken, I got permission denied." Using only what you learned in this lab, what would you tell them is already working, and what would you check next? *(Minimum 3 sentences.)*

```
I would tell them that the SSH connection is working because they were able to type in a password and get the "permission denied" error message. You can't get to the password step without being connected to SSH first. I would tell them to check their username, hostname, and password and make sure they type in everything correctly.
```

---

## Submission Checklist

- [x] Bastion path vs. manual SSH path described (Part A)

- [x] `ssh analyst@localhost` run and the first-connection prompt recorded (Part B, Steps 1–2)

- [x] Password entered; non-echoing input observed and described (Part B, Step 3)

- [x] `whoami`, `hostname`, `pwd` run inside the nested session (Part B, Step 4)

- [x] Prompt change described (Part B, Step 5)

- [x] `ssh-first-connection.png` captured, cropped, uploaded to `assets/screenshots/week-06/` (Part B, Step 6)

- [x] Session exited cleanly (Part B, Step 7)

- [x] Bad-username test run and `Permission denied` output recorded (Part C)

- [x] All four Analysis Questions answered (minimum sentence counts met)

- [x] This file is committed to your portfolio repo at `week-06/labs/lab-02-knocking-on-door-22.md`

---

## GitHub Commit Subsection

1. Open **Week 6 → Lab 02: Knocking on Door 22** in the Lab Portal.
2. Fill in the worksheet fields and upload `ssh-first-connection.png` to `assets/screenshots/week-06/`.
3. Click **Submit to GitHub** — the Portal commits to `week-06/labs/lab-02-knocking-on-door-22.md`.

---

*CyberVisionaries Institute · Cyber Foundations · Tier I*
