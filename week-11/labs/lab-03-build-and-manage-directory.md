# Lab 03 — Build and Manage the Directory

**Learner:** Angela Ousley  
**Attempt:** attempt E08803 · seed chic-w11-v1 · state revision 138 · text revision 939

> Training simulation — fictional Cloud Heights accounts and resources. Your actions change only your Week 11 learning attempt. This is not a live AD domain, Microsoft Entra tenant or Azure subscription.

## Objective

Create Jamie's AD account, place it, grant basic group access, and verify, reset and enable it safely.

## My answers

### What did OU placement change — and not change?

The OU placement changed what was listed in the DN (Distinguished Name) field, OU-Staff shows in both OU-STAFF and OU-SUP, but OU-Support only shows for OU-SUP. The group membership and credentials never changed regardless of the OU change.

### What did group membership change?

The group membership changed access depending on the group. The GA-SUP (Support-Team) didn’t grant access to anything because that group has no resource ACL entry. Adding Jamie to the GA-ALL (All-Staff) group granted her access to read the staff handbook.

### What did the credential reset change? Why did verification come first?

The credential reset changed/reset Jamie's password . Verification came first to ensure I am making changes to the correct account.

### What did disabling and re-enabling do to sessions and access?

Disabling and re-enabling the account changed the session numbers because status changes invalidates a session. Disabled accounts don't have sessions because they can't sign-in. Disabling an account revokes access to the system (can't sign-in) and re-enabling an account allows access to the system (can sign-in).

## Evidence

- **E05 · EV-E08803-006** — Created account with OU and groups (captured at revision 22)  
  _What this proves:_ It proves that an AD for Jamie was created with the OU-SUP and Staff and Support groups.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-006)
- **E06 · EV-E08803-007** — Passed verification + reset version change (captured at revision 24)  
  _What this proves:_ LA-E08803-23-1 proves that the staff ID CH-008 was verified to make sure the correct account is about to be changed. LA-E08803-24-1 proves that Jamies password was reset.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-007)
- **E07 · EV-E08803-008** — Disable / re-enable / session validation sequence (captured at revision 40)  
  _What this proves:_ Event 35-1 shows the account being disabled. 36-1 shows the failed sign-in and 37-1 shows the denied access test b/c the account is disabled. 38-1 shows the account being re-enabled. 39-1 shows a successful sign-in after being re-enabled. 40-1 shows access to read the handbook.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-008)
- **E05 · EV-E08803-009** — Created account with OU and groups (captured at revision 40)  
  _What this proves:_ 2nd capture. It proves that an AD account was created for Jamie. OU was changed, groups were added, credentials reset, and account status changed.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-009)
