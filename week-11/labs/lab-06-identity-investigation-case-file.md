# Lab 06 — Identity Investigation + Case File (Deliverable 4)

**Learner:** Angela Ousley  
**Attempt:** attempt E08803 · seed chic-w11-v1 · state revision 138 · text revision 969

> Training simulation — fictional Cloud Heights accounts and resources. Your actions change only your Week 11 learning attempt. This is not a live AD domain, Microsoft Entra tenant or Azure subscription.

## Objective

Correlate historical logs and assemble the Cloud Heights IAM Investigation Case File — Portfolio Deliverable 4.

## My answers

### Starting hypothesis before filtering

My hypothesis about the multiple sign-in attempts from casey@cvi-training.example is that the user forgot their password, or worse, an attacker could have been successful at guessing the password for the account.

## IAM Investigation Case File (Portfolio Deliverable 4)

_This is a technical case file. Week 12 uses it as source material to produce the professional Technical Incident Report and Executive Summary and to finalize and present the portfolio._

### 1. Scope and environment

This investigation looked into the Cloud Identity at user cl-casey. The user was removed from GC-PRIV group and added to the GC-RESP group. Access checks were performed to make sure Casey had the correct permissions. The IAM Ticket number (T402) was attached to each change made. The boundary is that this is all simulated. A user can't actually confirm that they can sign-in. A user can't verify that they have access to the resources that I assigned to them. The historical information related to Casey's account is limited.

### 2. Identity/directory trace and AD DS–Entra–Azure comparison

People, identities, accounts, groups, roles, and permissions all connect in a directory environment. A person is a human that access the resources through an account. An account is a record within a system that is connected to a person. A group is a collection of users. A role is used to bundle permissions together. Permissions allow a user to access specific resources.

### 3. Blair authentication-versus-authorization investigation

LS-E08803-4-1 shows cl-blair sign-in successful (authentication). AT-E08803-12-1 Blair was allowed to read case summaries (authorization). Authentication is proving you are who you say you are and authorization is having permission to access certain resources.

### 4. Directory administration and sensitive-action verification

LA-08803-23-1 shows an identity check passing for the ad-jamie account. LA-08803-24-1 , 25-1, and 27-1 shows a password reset for ad-jamie, an account status change from disabled to enabled, and a credential change. HR gave approval to make these changes in the directory via ticket T302.

### 5. Three job-role configurations, scopes and business rationales

1. Case Reader role (RL-READ). The scope is R-CASE and it's for people who read case summaries.
2. Response Agent role (RL-RESP). The scope is R-QUEUE and R-ABAC. They have permission to read/update the queue and read the response note.
3. Landing Reader role (RL-LAND) The scope is R-LAND. Can read the cloud case app page.

Each role is set with certain permissions that allows the employee just enough access to resources to do their job, that is least privilege.

### 6. Access test matrix and temporary grant/revoke evidence

LA-E08803-43-1 shows Blair being added to the audit group temporarily. AT-E08803-62-1 shows Blair being allowed Audit Evidence / read while being in the audit group. LA-E08803-63-1 shows Blair being removed from the audit group. AT-E08803-64-1 shows Blair being denied after her audit group access was removed.

### 7. ABAC current-policy and what-if findings

The ABAC current-policy is based on attributes (department, device trust, and classification). The what-if tests show what would happen if each attribute was changed individually. AT-08803-67-1 shows the device trust being changed to 'unmanaged' and Casey being denied access to Internal Response Note / read because the device isn't trusted. AT-08803-68-1 shows the department being changed to 'DEP-AUD' and Casey being denied access because the audit department doesn't have access to read DEP-RESP resources. AT-08803-69-1 shows the classification being changed to 'confidential' and Casey being denied access because her role on the Response team isn't allowed to read confidential information.

### 8. Joiner/mover/leaver and unverified-reset decisions

Joiner. Ticket T501 came from HR so I followed the request to enable Jamie's account and added her to the proper groups. I ran tests to make sure she had access to the resources assigned to her role.
Mover. Ticket T502 came from HR so I followed the request to move Alex from the response dept to the audit dept. I checked to make sure his old access was denied and that his audit access was allowed.
Leaver. Ticket T503 came from HR so I followed the request to disable all of Finley's accounts and remove him from groups. I made sure his access to resources were denied and that a sign-in from his account failed because his account was disabled.
Reset-request decision. Ticket T504 came from an unknown caller requesting a password reset for Blair's account. The identity verification failed so I did not initiate a password reset. The employee ID given by the caller did not match Blair's account, the ticket wasn't from HR, and the identity verification failed so I escalated the ticket because it was a suspicious request.

### 9. Historical identity findings (see findings below)

The historical finding that I referred to for Casey's failed sign-in investigation is A002. Casey's account was added to the Privilege Operators group after 5 failed sign-in attempts and a successful 6th sign-in attempt. The correlation ID C009 associates this historical finding to the S009 successful sign-in.

#### Finding 1: Five Failed Sign-in Attempts and Success on the Sixth Attempt

- **Events:** S004, S005, S006, S007, S008, S009
- **Observation:** The records show that Casey's account failed authentication 5 times before getting authenticated on the 6th sign-in attempt.
- **Hypothesis:** Casey might have forgotten their password or may have be hacked.
- **Supported conclusion:** Not able to form a conclusion based on the current evidence.
- **Uncertainty / missing information:** It doesn't tell me if it was actually Casey signing-in.  The device ID used to sign-in is unknown.
- **Next action:** Check Casey's previous sign-in history if possible and see if the IP address and location of the failed sign-in attempts matches with her known history. MFA should be enabled for Casey's account, right now it's just password only to authenticate. Revoking casey@cvi-training.example current session might be a good idea to force the user to re-authenticate.
- **Priority:** high — High priority because 5 failed login attempts is suspicious. The device used to sign-in is 'unknown' and MFA wasn't used to authenticate.

#### Finding 2: Casey Added to Privilege Operators Group w/o Approval Ticket

- **Events:** A002
- **Observation:** Audit record shows that Casey's account was added to a privileged group, but there wasn't a ticket request for Casey's account to receive such changes.
- **Hypothesis:** User added themselves without proper approval or a hacker is leveling up  privileges to gain access to broad areas of the system.
- **Supported conclusion:** Can't come to a conclusion yet, not enough information.
- **Uncertainty / missing information:** Why did the user add themselves to a high level group? Was this done by Casey or an unauthorized user.
- **Next action:** Look at the Cloud Identity and see what department Casey belongs to. See if there's a ticket requesting privilege authorization for Casey's account. If a proper request for this type of change isn't found, remove Casey's account from the Privilege Operators group. Revoke the session so the user will have to re-authenticate.
- **Priority:** high — Need to figure out if the group access change was made with permission via ticket, if it was done by Casey, or if an attacker has gained access to the CaseDesk application. A privileged user can read/update/delete all the records in the system.

### 10. Sign-in/audit correlation and one benign/ambiguous comparison

The correlation ID C009 connects the S009 sign-in and the A002 audit records. It shows that right after sign-in happened, Casey's account was added to the Privilege Operators group, giving her higher permissions than she already had. No IT ticket was assigned to this change.

**Benign/ambiguous comparison** (S004, S005, S006, S007, S008, S009, A002): The five failed login attempts looks unusual along with the privilege group addition after finally getting signed in on the sixth sign-in attempt. It's possible the user entered in the incorrect password 5 times before finally remembering it. These records aren't enough to claim malicious activity because it's not enough evidence to do so.

**Live simulator chain (current attempt, not historical):** Based on ticket T402, I removed Casey (cl-casey) from the Privileged Operators group (GC-PRIV) and added her to the Response Operators group (GC-RESP), LA-08803-47-1 and 48-1 audit records show this. I tested that she could read and update resources in the R-QUEUE (AT-08803-53-1 and 54-1). I tested that she could no longer access audit and admin resources with R-AUD and R-DIR (AT-08803-55-1 and 56-1). LA-E08803-47-1 LA-E08803-48-1 LA-E08803-53-2 LA-E08803-54-2 LA-E08803-55-2 LA-E08803-56-2 AT-E08803-53-1 AT-E08803-54-1 AT-E08803-55-1 AT-E08803-56-1 LS-E08803-52-1

### 11. Preliminary response/remediation recommendations, priority and uncertainty

Check to see if Casey signed in from that IP address before. Check to see if there's a proper ticket requesting for her to have privileged permissions. Next actions are to enable MFA for her account, revoke the current session so that the user will be forced to re-authenticate, and remove Casey's account from the Privileged Operators group if there isn't a ticket for her to have access. What remains uncertain is what device was used to login into Casey's account, has she logged in from this IP address before, and if the user is really Casey or not.

### 12. Evidence index notes

E17 and E18 show the historical sign-in and audit events S004, S005, S006, S007, S008, S009, A002. 
S004-009 is used in Finding 1 to show the 5 failed login attempts and the 6th successful login using an unknown device.
A002 is used to show the privilege escalation that took place 1 minute after the successful sign-in.

### Explicit simulation/environment limitations

The available records don't tell me if Casey has logged in using this IP address before. The available tickets don't show me that she should be a privileged operator. No activity is shown after Casey's account was added to the privilege group, so we don't know what changes might have been made after that.

### Earlier labs

- [Lab 01](lab-01-identity-directory.md)
- [Lab 02](lab-02-authentication-vs-authorization.md)
- [Lab 03](lab-03-build-and-manage-directory.md)
- [Lab 04](lab-04-least-privilege-challenge.md)
- [Lab 05](lab-05-joiner-mover-leaver.md)

## Evidence

- **E17 · EV-E08803-010** — Two distinct findings with raw-event references and correlation (captured at revision 42)  
  _What this proves:_ Records S004 - S008 show that authentication failed 5 times. S009 shows a successful login using Casey's account. A002 shows Casey's account adding itself to the Privilege Operators group. Cl-casey cloud identity snapshot shows that he/she is part of the Response department and the current groups and permissions the account has. Ticket T402 shows the permissions that are supposed to be applied to Casey's account. The current evidence shows that Casey's account was added to a group without having a ticket and that the current ticket associated with Casey's account hasn't been implemented yet. The account has privileges it's not supposed to have according to the ticket and also has some missing permissions according to the ticket. None of this can establish if Casey made these changes or was hacked.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-010)
- **E17 · EV-E08803-024** — Two distinct findings with raw-event references and correlation (captured at revision 136)  
  _What this proves:_ Records S004-S008 show the failed sign-in attempts. S009 shows the successful sign-in and has correlation ID C009 and is related to A002 that shows Casey being added to the Privileged Operators group.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-024)
- **E18 · EV-E08803-025** — Benign/ambiguous comparison (captured at revision 136)  
  _What this proves:_ The records show some suspicious activity with failed login attempts and then privilege escalation. But the records don't prove that there is malicious activity. More evidence is needed to come to a conclusion.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-025)
- **E19 · EV-E08803-026** — Completed case file / versioned export (captured at revision 138)  
  _What this proves:_ Sign-in records S004-S008 show the failed sign-in attempts. S009 shows the successful sign-in. A002 shows that right after sign-in happened, Casey's account was added to the Privilege Operators group, giving her higher permissions than she already had. No IT ticket was assigned to this change.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-026)
