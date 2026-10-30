# Cloud Heights Identity Investigation — Technical Incident Report

> Fictional training scenario: Cloud Heights Identity Center (simulated). No real organisation, people or systems.

- Analyst: Angela Ousley
- Source case file: [../week-11/labs/lab-06-identity-investigation-case-file.md](../week-11/labs/lab-06-identity-investigation-case-file.md)
- Evidence index: [../week-11/labs/evidence/week11-evidence-index.md](../week-11/labs/evidence/week11-evidence-index.md)

## 1. Purpose / incident context
This report documents the investigation of an employee's cloud account. Suspicious login attempts were made using the account and then an immediate privilege escalation was initiated. This sequence of activity requires further investigation.

## 2. Scope
Cloud account "casey@cvi-training.example" is under investigation due to its historical activity. Sign-in and audit logs show odd activity on 2026-09-21. The logs on this day are the earliest records that I have to refer to for this account.

## 3. Evidence reviewed
Sign-in and audit log records were used to investigate. EV17 shows the evidence captured.

Evidence IDs (EV-…) and event IDs (S…, A…, C…) refer to records in my Week 11 evidence index; full evidence is not duplicated here.

## 4. Timeline
event_id	time_utc                                  user                                                  source_ip              device_id                   outcome      reason
S004          2026-09-21T14:00:00Z    casey@cvi-training.example     203.0.113.77      unknown-device      failure          incorrect_password
S005          2026-09-21T14:00:30Z    casey@cvi-training.example     203.0.113.77      unknown-device      failure          incorrect_password
S006          2026-09-21T14:01:00Z    casey@cvi-training.example     203.0.113.77      unknown-device      failure          incorrect_password
S007          2026-09-21T14:01:30Z    casey@cvi-training.example     203.0.113.77      unknown-device      failure          incorrect_password
S008          2026-09-21T14:02:00Z    casey@cvi-training.example     203.0.113.77      unknown-device      failure          incorrect_password
S009          2026-09-21T14:03:00Z    casey@cvi-training.example     203.0.113.77      unknown-device      success       accepted

event_id	time_utc                                    actor                                                activity                                 target                                               after_value                       approval_ticket
A002         2026-09-21T14:04:00Z       casey@cvi-training.example    group_member_added    casey@cvi-training.example    Privileged-Operators     none_recorded

## 5. Findings
### Finding 1 (RF1) — confidence: confirmed

The records S004 - S009 show that Casey's account had 5 failed login attempts and then a successful login on the 6th attempt. This sequence is suspicious and deserves further investigation.

- Evidence / events: S004, S005, S006, S007, S008, S009
- Uncertainty: The records don't confirm if it was actually Casey signing-in.  The device ID used to sign-in is unknown.

### Finding 2 (RF2) — confidence: confirmed

Record A002 shows that Casey's account was added to the Privileged Operators group one minute after the successful login. I was unable to confirm if this change was authorized.

- Evidence / events: A002
- Uncertainty: The records don't show if this change was authorized. Why did the user add themselves to a high level group? Was this done by Casey or an unauthorized user?

## 6. Impact / risk
If this change was not authorized, it will allow Casey's account to make major changes within the cloud. All users could be affected. The 'casey@cvi-training.example' account being added to the Privileged Operators group gives the account high level permissions to read, update, and delete all the resources in the system. That level of permission can also manage groups and users, so other users in the system could loss access to their currently assigned groups and resources. Casey's account having that level of access controls everything in the cloud.

## 7. Recommendations / next actions
### Recommendation 1 (R1)

- **What:** 1. Revoke the current session
2. Immediately change the password word for account casey@cvi-training.example
3. Add MFA to the account for a more secure authentication process
4. Verify that the account being added to the Privilege-Operators group was an approved change 
5. Verify that the account has the correct access based on whether the change was authorized or not
- **Who:** IAM Team
- **Why:** These actions lower the risk of major changes being made in the cloud (such as reading, updating, or deleting resources, groups, and users) through an account that may or may not authorized to do so.
- **Priority:** high
- **Timeframe:** Immediately. Within the next 4 hours at the latest.
- **Linked findings:** RF1, RF2
- **Success measure:** 1. Check the audit log to verify the latest session for casey@cvi-training.example is revoked
2. Check the audit log to verify the password has been reset for casey@cvi-training.example
3. Check the audit log to verify MFA enrollment for casey@cvi-training.example
4. An IAM ticket, email, or message from HR or Casey's manager proving that casey@cvi-training.example being added to the Privilege Operators group was an approved change 
5. Verify the cloud identity for account 'cl-casey' has the correct groups listed in the 'Groups and Effective Access' table 
6. Run tests to make sure the account has access to the resources in which its group is assigned to
7. Run tests to make sure the account does NOT have access to resources for groups it is not assigned to

## 8. Limitations / unknowns
This investigation is limited by a few historical records. The available records don't tell me if Casey has logged in using this IP address before. The available tickets don't show me that she should be a privileged operator. No activity is shown after Casey's account was added to the privilege group, so we don't know what changes might have been made after that.

## 9. Conclusion
Based on the available records I am unable to determine if the user was actually Casey signing in or if the privilege escalation was authorized. More information is needed to come to a reasonable determination.
