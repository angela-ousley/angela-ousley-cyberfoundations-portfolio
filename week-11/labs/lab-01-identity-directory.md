# Lab 01 — Identity Directory: Who Is Who?

**Learner:** Angela Ousley  
**Attempt:** attempt E08803 · seed chic-w11-v1 · state revision 138 · text revision 939

> Training simulation — fictional Cloud Heights accounts and resources. Your actions change only your Week 11 learning attempt. This is not a live AD domain, Microsoft Entra tenant or Azure subscription.

## Objective

Investigate why one person may have different access in different identity systems.

## My answers

### Difference 1 between the AD and cloud views (what you saw, and why it matters)

I noticed that the cloud view for "Roles and Access" grants permissions by role assignment and access by the group assigned. The AD view assigns access and permissions based on the group. This matters because someone who is assigning access and permissions will need to remember the difference between the two directories (cloud vs AD).

### Difference 2 between the AD and cloud views

The second difference I noticed between the AD and cloud views is that the cloud does not have OUs (organizational units) like the active directory has.

### What do Junior-Analysts and Case-Readers actually grant? How did you check?

The Junior-Analysts group grants reading access to the app_page (Analyst Landing). The Case-Readers group grants reading access to case summaries. I clicked on "Groups" and looked at the 'Grants' section for each group which listed the Role and the Scope (resource). Then I clicked on "Roles & Access" to see the permissions for each role.

### Why does Drew's person record still exist?

Drew's record still exist for accountability purposes. Records must still remain for a leaver for  auditing purposes.

## Directory trace

| Concept | Object |
|---|---|
| ou | OU-DIS Disabled Accounts |
| role | RL-RESP Response Agent @ R-QUEUE |
| group | GA-RESP Response-Team |
| person | P01 Alex Morgan |
| account | ad-alex (alex@ad.cloudheights.example) |
| identity | P02 Blair Chen |
| permission | queue → read |

## Evidence

- **E01 · EV-E08803-001** — Directory trace: Alex person, both accounts, OU, one AD grant path, one cloud role path (captured at revision 1)  
  _What this proves:_ It proves that Alex has two separate accounts, one in active directory (AD) and the other in the cloud (Microsoft Entra ID).  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-001)
- **E02 · EV-E08803-002** — Comparison of Blair, Finley and Drew (captured at revision 3)  
  _What this proves:_ It proves that Blair, Finley, and Drew all have two separate accounts (one in the cloud and one in AD). It proves that Drew's accounts are disabled and that he isn't granted access to any of the resources.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-002)
- **E02 · EV-E08803-003** — Comparison of Blair, Finley and Drew (captured at revision 3)  
  _What this proves:_ It proves that Blair, Finley, and Drew all have two separate accounts (one in the cloud and one in AD). It proves that Drew's accounts are disabled and that he isn't granted access to any of the resources.  
  See [evidence index](evidence/week11-evidence-index.md#ev-e08803-003)
