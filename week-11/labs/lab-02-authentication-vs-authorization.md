# Lab 02 — Authentication vs. Authorization: A Valid Badge Doesn't Open Every Door

**Learner:** Angela Ousley  
**Attempt:** attempt E08803 · seed chic-w11-v1 · state revision 138 · text revision 939

> Training simulation — fictional Cloud Heights accounts and resources. Your actions change only your Week 11 learning attempt. This is not a live AD domain, Microsoft Entra tenant or Azure subscription.

## Objective

Diagnose and repair Blair's missing case access without granting update rights.

## My answers

### Prediction before testing: will Blair be able to read cases? Why?

Blair will not be able to read cases because she doesn't have access to the Case Summaries group.

### Identification — using the actual account

Event_id A001 in the Audit Logs is where Blair's blair@cvi-training.example account is added to the group Junior-Analysts.

### Authentication — using your sign-in event

Event_id S003 in the Sign-in Logs is where Blair (blair@cvi-training.example) gets authenticated into the CaseDesk application using a password and MFA.

### Authorization — using your access tests

Event_id LA-E08803-12-2	in the Audit Logs is where the access test shows that Blair is able to read Case Summaries (R-CASE).

### Accountability — using the audit record

Before Blair was granted access to the Case Summaries group, event_id LA-E08803-5-2	 shows an access tests with a "deny" outcome because Blair wasn't added to the "GC-READ" Case-Readers group yet. Every audit record shows accountability for actions taken in the system.

## Evidence

- **E03 · EV-E08803-004** — Successful sign-in + the access result before your change (captured at revision 16)  
  _What this proves:_ The LS-E08803-4-1 event proves that Blair was able to sign-in using her account. The AT-E08803-5-1 access test proves that Blair did not have access to read the Case Summaries and was denied when she tried to access it.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-004)
- **E04 · EV-E08803-005** — Membership/access change audit + read result + update result (captured at revision 16)  
  _What this proves:_ The LA-E08803-11-1 audit event shows Blair's account being added to GC-READ (Case Summaries) group. The AT-E08803-12-1 shows the retesting of reading the Case Summaries and her being allowed access. The AT-E08803-13-1 shows Blair being denied when trying to update the Case Summaries, which is expected because she only has read access.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-005)
