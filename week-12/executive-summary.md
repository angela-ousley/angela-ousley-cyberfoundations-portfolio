# Executive Summary — Cloud Heights Identity Investigation

> Fictional training scenario: Cloud Heights Identity Center (simulated). No real organisation, people or systems.

- Prepared by: Angela Ousley
- Audience: Cloud Heights leadership
- Length: 232 words (target about 250–400)
- Full technical detail: [technical-incident-report.md](technical-incident-report.md)

## What happened?
There is suspicious activity associated with a user's cloud account. There were failed login attempts and then a privilege escalation right after a successful sign-in.

## Why does it matter?
This matters because we need to be sure that the privilege change was authorized. This account now has access to all of the resources in the cloud such as audit files, case summaries, response queue, and the directory. This level of access means the user can read, update, or delete any of the resources, groups, and users.

## What did we find?
Casey's cloud account had 5 failed login attempts from an unknown device. The 6th login attempt was successful and a privilege escalation was applied to that account one minute after the successful sign-in. I am unable to locate an IAM ticket for this change, therefore this situation requires further investigation.

## What should happen next?
Immediate action should be taken by the IAM team to reset the password for the account and revoke the current session so that no further changes can be made by the account. If the approval for the group change (privilege escalation) to Casey's cloud account remains unresolved, the IAM team should verify if this change was authorized or not and review the access that the account currently has. The IAM team should also enable MFA for Casey's account as another layer of security.

## What remains uncertain
Is there an IAM ticket for the change to this account? Was the change made by Casey?
