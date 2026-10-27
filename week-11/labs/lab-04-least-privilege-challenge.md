# Lab 04 — Least Privilege Challenge

**Learner:** Angela Ousley  
**Attempt:** attempt E08803 · seed chic-w11-v1 · state revision 138 · text revision 939

> Training simulation — fictional Cloud Heights accounts and resources. Your actions change only your Week 11 learning attempt. This is not a live AD domain, Microsoft Entra tenant or Azure subscription.

## Objective

Configure three job-role assignments, remove excess access, and test role-based and attribute-based access.

## My answers

### Blair, Casey, Emi: job, required actions, forbidden actions

Blair: Junior Analyst. Read case summaries only. Temporary audit-access for training. Can’t view anything beyond the landing page.
Casey: Response Operator. Read and update the response queue; read Internal Response Note. No audit deletion and no directory privileges.
Emi: Audit Reviewer. Read and export audit evidence. Can’t delete anything.

### Prediction for the three what-if cases, then which attribute changed each result

I have no prediction for the what-if cases. The department, device, and classification override cases doesn't really explain much to me. Each attribute being changed individually (department, device, classification) all gave a deny result when trying to read the internal response note while in Casey's account.

### RBAC and assignment scope in your own words

RBAC (role based access control) is being granted permissions based on the job position/job title. The assignment scope is the resource or application that is applied to a role.

### Least privilege — what you removed and why

Least privilege is only having the access you need to be able to do your job. I removed membership to groups that weren't needed for certain users to do their assigned task, allowing them enough permissions to do their jobs.

### Why sign-in didn't settle authorization, and how ABAC adds conditions

Sign-in didn't settle authorization because sigin-in is associated with authentication (proving who you are), not authorization (what you're allowed to access). ABAC (attribute-based access control) adds conditions by using policy to evaluate attributes of a request. Casey was allowed to read the internal response note on just RBAC, but once ABAC was added in, she was denied in each change of one of the attributes.

## Evidence

- **E08 · EV-E08803-011** — Three job configuration snapshots with rationales (captured at revision 46)  
  _What this proves:_ The 43-1 configuration shows me adding Blair to GC-AUD b/c ticket 401 said she needed "temporary audit-access for training". The 44, 45, and 46 configurations show no changes because the tickets are for setting up access for the employees, not to change the permissions of the group or role.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-011)
- **E08 · EV-E08803-012** — Three job configuration snapshots with rationales (captured at revision 58)  
  _What this proves:_ Evidenc 08 Part 2: Please refer to E08 EV-E08803-011 for cl-blair and cl-casey. I had to update step 14 and add Emi to the GC-AUD. I was confused about the instructions at first, but have corrected her access by adding her to the Audit group.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-012)
- **E09 · EV-E08803-013** — The nine required access tests (captured at revision 61)  
  _What this proves:_ The records show the expected allow/deny access based on the group/role the user is assigned to.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-013)
- **E10 · EV-E08803-014** — Temporary grant → allowed test → revoke → denied retest (captured at revision 64)  
  _What this proves:_ LA-E08803-43-1 and 63-1 shows Blair being added to the audit group temporarily and then being removed from the audit group.  AT-E08803-62-1 and 64-1 shows Blair being allowed while having access and then being denied once her access was removed.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-014)
- **E11 · EV-E08803-015** — Four ABAC cases (current + three what-ifs) (captured at revision 69)  
  _What this proves:_ The current state shows Casey being allowed access to read the internal response note. The three what-if cases show each individual attribute being changed and all three what-if cases giving a deny result when trying to read the internal response note.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-015)
