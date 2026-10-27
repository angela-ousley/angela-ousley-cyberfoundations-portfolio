# Lab 05 — Joiner, Mover, Leaver

**Learner:** Angela Ousley  
**Attempt:** attempt E08803 · seed chic-w11-v1 · state revision 138 · text revision 939

> Training simulation — fictional Cloud Heights accounts and resources. Your actions change only your Week 11 learning attempt. This is not a live AD domain, Microsoft Entra tenant or Azure subscription.

## Objective

Complete four lifecycle tickets across the two directory surfaces, plus a short Azure scope comparison.

## My answers

### T501 decision table — Add / Retain / Remove / Deny / Escalate (write 'not applicable — reason' where needed)

Jamie was added to the junior analyst and case reader groups per the ticket request. Access is functioning as expected based on tests for reading/updating the case summary.

### T502 decision table — Add / Retain / Remove / Deny / Escalate

Alex was transferred from the Response department to the Audit department, moved from the response OU to the audit OU, and removed from the response group while added to the audit group. Testing confirms his permissions are working as expected.

### T503 decision table — Add / Retain / Remove / Deny / Escalate

Contractor Finley was terminated. The ad-finley and cl-finley accounts are now disabled, all sessions are ended, memberships are removed, the AD account is in the disabled accounts list, and nothing was deleted.

### T504 decision table — Add / Retain / Remove / Deny / Escalate

Blair's password was not reset because the identity verification step failed. The employee ID given by the requestor (CH-099) does not match the HR record for Blair (CH-002). The ticket has been escalated for further investigation.

### Why is an Azure resource role/scope different from a cloud directory role?

They are two separate systems. The Azure resource in this scenario is a virtual machine (VM). A cloud directory handles identities (users & groups). The Azure resource role/scope controls different layers than a cloud directory role.

## Evidence

- **E12 · EV-E08803-016** — Jamie joiner chain (captured at revision 83)  
  _What this proves:_ Creation of Jamie's account (73), MFA enrollment (75), Status changed to 'enabled (76), adding user to junior analyst and case reader groups (80 & 81), test for read case summary was allowed, and the test for update case summary was denied as expected. Both group have read only permissions.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-016)
- **E13 · EV-E08803-017** — Alex before/after access and changes (captured at revision 98)  
  _What this proves:_ Showing the current state of Alex's cloud and AD accounts before making changes listed on the T502 ticket. Right now he still has access to read the response share and response queue b/c the role transfer hasn't been initiated yet.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-017)
- **E13 · EV-E08803-018** — Alex before/after access and changes (captured at revision 111)  
  _What this proves:_ Showing ad-alex and cl-alex accounts after moving him to the audit group (before is shown in the previous E13). Tests 96 and 98 show testing before he was moved to the audit group. The rest of the tests show allow and deny responses after being moved to the audit group and removed from the response group. The audit records for the changes made to the account show the department, OU, and group changes made to the cloud and AD accounts.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-018)
- **E14 · EV-E08803-019** — Finley session/status/revocation evidence (captured at revision 124)  
  _What this proves:_ Records LA-E08803-115-1 and 119 show ad-finley and cl-finley being disabled and the cl-finley session being revoked. Records LA-E08803-116-1 and 120 show that no sessions were revoked because ad-finley didn't have an open session and the cl-finley session automatically revoked when the account was disabled. The retest with the old session account (AT-E08803-123-1) was denied b/c the account is disabled. The sign-in attempt (LS-E08803-124-1) failed b/c the account is disabled.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-019)
- **E15 · EV-E08803-020** — Failed reset verification, refusal and escalation (captured at revision 126)  
  _What this proves:_ The LA-E08803-125-1  record shows the identity verification failing. The password was not reset to due identification failure. The LA-E08803-126-1 record shows the ticket being escalated for further investigation.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-020)
- **E16 · EV-E08803-021** — AD DS – Entra – Azure comparison and scoped tests (captured at revision 132)  
  _What this proves:_ Record LA-E08803-127-1 shows Blair being added to RG-LAB, LA-E08803-128-1 shows her being added to GC-AZ. Records AT-E08803-130-1, 131-1, and 132-1 shows that Blair can read metadata, but can't stop the VM, and can't read the 'other' team's VM data.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-021)
- **E13 · EV-E08803-022** — Alex before/after access and changes (captured at revision 135)  
  _What this proves:_ Showing ad-alex and cl-alex accounts after moving him to the audit group (before is shown in the previous E13). Tests 96 and 98 show testing before he was moved to the audit group. The rest of the tests show allow and deny responses after being moved to the audit group and removed from the response group. The audit records for the changes made to the account show the department, OU, and group changes made to the cloud and AD accounts. Added AT-E08803-135-1 to the evidence to show that Alex can export audit evidence.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-022)
- **E14 · EV-E08803-023** — Finley session/status/revocation evidence (captured at revision 136)  
  _What this proves:_ Records LA-E08803-115-1 and 119 show ad-finley and cl-finley being disabled and the cl-finley session being revoked. Records LA-E08803-116-1 and 120 show that no sessions were revoked because ad-finley didn't have an open session and the cl-finley session automatically revoked when the account was disabled. LA-E08803-118-1, 121-1, and 136-1 removed Finley from GA-ALL, GC-READ, and GA-SUP groups. The retest with the old session account (AT-E08803-123-1) was denied b/c the account is disabled. The sign-in attempt (LS-E08803-124-1) failed b/c the account is disabled.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-023)
