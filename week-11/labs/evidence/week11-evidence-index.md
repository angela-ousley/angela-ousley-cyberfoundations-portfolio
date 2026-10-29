# Week 11 evidence index

Simulator captures from attempt E08803. Hash detects content changes; it is not proof of live AD administration.

## EV-E08803-001

- Slot E01 · M01 · revision 1
- Directory trace: Alex person, both accounts, OU, one AD grant path, one cloud role path
- SHA-256 566bee62701f7da293880123af3cb4ab3f78991b417cde0d9b28e11a22d4934d
- Caption: It proves that Alex has two separate accounts, one in active directory (AD) and the other in the cloud (Microsoft Entra ID).

## EV-E08803-002

- Slot E02 · M01 · revision 3
- Comparison of Blair, Finley and Drew
- SHA-256 e52a3ca1a359ebf796abb510bb15465dfa602e930389abff5db4ac289f11777b
- Caption: It proves that Blair, Finley, and Drew all have two separate accounts (one in the cloud and one in AD). It proves that Drew's accounts are disabled and that he isn't granted access to any of the resources.

## EV-E08803-003

- Slot E02 · M01 · revision 3
- Comparison of Blair, Finley and Drew
- SHA-256 1cfc6b1a3fe2c3f5b2b55d10f57ab93d47d8a93baac6cce0d8a9c4df739cbadc
- Caption: It proves that Blair, Finley, and Drew all have two separate accounts (one in the cloud and one in AD). It proves that Drew's accounts are disabled and that he isn't granted access to any of the resources.

## EV-E08803-004

- Slot E03 · M02 · revision 16
- Successful sign-in + the access result before your change
- SHA-256 21eedb42ce4b1074542040f592f554bfc9ad0cda84f76d3cc5632598212c06d6
- Caption: The LS-E08803-4-1 event proves that Blair was able to sign-in using her account. The AT-E08803-5-1 access test proves that Blair did not have access to read the Case Summaries and was denied when she tried to access it.

## EV-E08803-005

- Slot E04 · M02 · revision 16
- Membership/access change audit + read result + update result
- SHA-256 460fab11f296675b8090cf970aa8b1f117d9b7d036f6129f3ef3a3a2d0e3165d
- Caption: The LA-E08803-11-1 audit event shows Blair's account being added to GC-READ (Case Summaries) group. The AT-E08803-12-1 shows the retesting of reading the Case Summaries and her being allowed access. The AT-E08803-13-1 shows Blair being denied when trying to update the Case Summaries, which is expected because she only has read access.

## EV-E08803-006

- Slot E05 · M03 · revision 22
- Created account with OU and groups
- SHA-256 9cef2ec8a99f1411507186eece0ef00342f859dbc489276c8071bf0a4f806e60
- Caption: It proves that an AD for Jamie was created with the OU-SUP and Staff and Support groups.

## EV-E08803-007

- Slot E06 · M03 · revision 24
- Passed verification + reset version change
- SHA-256 8117c27f14b79d1241d1719fd63d86e6cca7d66bb3a3b853de8a89f5a65d5cfa
- Caption: LA-E08803-23-1 proves that the staff ID CH-008 was verified to make sure the correct account is about to be changed. LA-E08803-24-1 proves that Jamies password was reset.

## EV-E08803-008

- Slot E07 · M03 · revision 40
- Disable / re-enable / session validation sequence
- SHA-256 e3184ecd9d5f68d2e9c0cee6eb650716fca529db9adf90a61582d3cf50c1a70d
- Caption: Event 35-1 shows the account being disabled. 36-1 shows the failed sign-in and 37-1 shows the denied access test b/c the account is disabled. 38-1 shows the account being re-enabled. 39-1 shows a successful sign-in after being re-enabled. 40-1 shows access to read the handbook.

## EV-E08803-009

- Slot E05 · M03 · revision 40
- Created account with OU and groups
- SHA-256 4a491458ceecd602f892032b89301ed55486d87b9cc94483c8893bf592458482
- Caption: 2nd capture. It proves that an AD account was created for Jamie. OU was changed, groups were added, credentials reset, and account status changed.

## EV-E08803-010

- Slot E17 · M06 · revision 42
- Two distinct findings with raw-event references and correlation
- SHA-256 596bbdf86108cbcb69db67779e390ed64a0953f874916bfa8b3f4f734702637f
- Caption: Records S004 - S008 show that authentication failed 5 times. S009 shows a successful login using Casey's account. A002 shows Casey's account adding itself to the Privilege Operators group. Cl-casey cloud identity snapshot shows that he/she is part of the Response department and the current groups and permissions the account has. Ticket T402 shows the permissions that are supposed to be applied to Casey's account. The current evidence shows that Casey's account was added to a group without having a ticket and that the current ticket associated with Casey's account hasn't been implemented yet. The account has privileges it's not supposed to have according to the ticket and also has some missing permissions according to the ticket. None of this can establish if Casey made these changes or was hacked.

## EV-E08803-011

- Slot E08 · M04 · revision 46
- Three job configuration snapshots with rationales
- SHA-256 bfc02637c548775794e4fb5317291c10f94dc136a53fee288772b36c41aae364
- Caption: The 43-1 configuration shows me adding Blair to GC-AUD b/c ticket 401 said she needed "temporary audit-access for training". The 44, 45, and 46 configurations show no changes because the tickets are for setting up access for the employees, not to change the permissions of the group or role.

## EV-E08803-012

- Slot E08 · M04 · revision 58
- Three job configuration snapshots with rationales
- SHA-256 68f06accfb7a79032bf8cb91108fa94bfd15f05b9cc25bf1aaac60ffc8c45460
- Caption: Evidenc 08 Part 2: Please refer to E08 EV-E08803-011 for cl-blair and cl-casey. I had to update step 14 and add Emi to the GC-AUD. I was confused about the instructions at first, but have corrected her access by adding her to the Audit group.

## EV-E08803-013

- Slot E09 · M04 · revision 61
- The nine required access tests
- SHA-256 710b56b24b62727641be52270be5a4e45911b253fb048bc227b0e1b5dd21b5e3
- Caption: The records show the expected allow/deny access based on the group/role the user is assigned to.

## EV-E08803-014

- Slot E10 · M04 · revision 64
- Temporary grant → allowed test → revoke → denied retest
- SHA-256 cd90f708701af31bb19d3a4b0a9718f925ab6067730a489d254df8ab5b686643
- Caption: LA-E08803-43-1 and 63-1 shows Blair being added to the audit group temporarily and then being removed from the audit group.  AT-E08803-62-1 and 64-1 shows Blair being allowed while having access and then being denied once her access was removed.

## EV-E08803-015

- Slot E11 · M04 · revision 69
- Four ABAC cases (current + three what-ifs)
- SHA-256 60b4b604fb1f98a86d1a60cf2ada2617b9a010486bfe8d6d1e69244dc08a7ef3
- Caption: The current state shows Casey being allowed access to read the internal response note. The three what-if cases show each individual attribute being changed and all three what-if cases giving a deny result when trying to read the internal response note.

## EV-E08803-016

- Slot E12 · M05 · revision 83
- Jamie joiner chain
- SHA-256 20f63eb0504d75ba06d7968a9e09cfdf6d99653a6bdb0d21c0bc8d1bd4e48fdc
- Caption: Creation of Jamie's account (73), MFA enrollment (75), Status changed to 'enabled (76), adding user to junior analyst and case reader groups (80 & 81), test for read case summary was allowed, and the test for update case summary was denied as expected. Both group have read only permissions.

## EV-E08803-017

- Slot E13 · M05 · revision 98
- Alex before/after access and changes
- SHA-256 c84724c1995ea084b3bba2f00d06cc2ea0bd3b4864b675e16c92f87fc677c11e
- Caption: Showing the current state of Alex's cloud and AD accounts before making changes listed on the T502 ticket. Right now he still has access to read the response share and response queue b/c the role transfer hasn't been initiated yet.

## EV-E08803-018

- Slot E13 · M05 · revision 111
- Alex before/after access and changes
- SHA-256 57ed2bd01b6096a7d49827d40a3141a9107be46c7e1d444947d7725d817314ca
- Caption: Showing ad-alex and cl-alex accounts after moving him to the audit group (before is shown in the previous E13). Tests 96 and 98 show testing before he was moved to the audit group. The rest of the tests show allow and deny responses after being moved to the audit group and removed from the response group. The audit records for the changes made to the account show the department, OU, and group changes made to the cloud and AD accounts.

## EV-E08803-019

- Slot E14 · M05 · revision 124
- Finley session/status/revocation evidence
- SHA-256 86def00cb082a6b7a0a3eb21412464226fd8d736cc02257cba596f4e586ebfc5
- Caption: Records LA-E08803-115-1 and 119 show ad-finley and cl-finley being disabled and the cl-finley session being revoked. Records LA-E08803-116-1 and 120 show that no sessions were revoked because ad-finley didn't have an open session and the cl-finley session automatically revoked when the account was disabled. The retest with the old session account (AT-E08803-123-1) was denied b/c the account is disabled. The sign-in attempt (LS-E08803-124-1) failed b/c the account is disabled.

## EV-E08803-020

- Slot E15 · M05 · revision 126
- Failed reset verification, refusal and escalation
- SHA-256 bb8112428d9b4e423017a65e32f187c95c8f7ad3a34b10ccead1d6f9647ffb99
- Caption: The LA-E08803-125-1  record shows the identity verification failing. The password was not reset to due identification failure. The LA-E08803-126-1 record shows the ticket being escalated for further investigation.

## EV-E08803-021

- Slot E16 · M05 · revision 132
- AD DS – Entra – Azure comparison and scoped tests
- SHA-256 6efbdfe5fb520c1d02dfe125bdd3a6f711918453e653242b0dab099013330eca
- Caption: Record LA-E08803-127-1 shows Blair being added to RG-LAB, LA-E08803-128-1 shows her being added to GC-AZ. Records AT-E08803-130-1, 131-1, and 132-1 shows that Blair can read metadata, but can't stop the VM, and can't read the 'other' team's VM data.

## EV-E08803-022

- Slot E13 · M05 · revision 135
- Alex before/after access and changes
- SHA-256 20aaf537e545ee4e914758a4a81ce78fa5e4bdab7b82306c428b4f4a30206815
- Caption: Showing ad-alex and cl-alex accounts after moving him to the audit group (before is shown in the previous E13). Tests 96 and 98 show testing before he was moved to the audit group. The rest of the tests show allow and deny responses after being moved to the audit group and removed from the response group. The audit records for the changes made to the account show the department, OU, and group changes made to the cloud and AD accounts. Added AT-E08803-135-1 to the evidence to show that Alex can export audit evidence.

## EV-E08803-023

- Slot E14 · M05 · revision 136
- Finley session/status/revocation evidence
- SHA-256 5cd527a6c624eeed97a7e843d26fe105ed4ee5a28aa88bd162c660480980db1d
- Caption: Records LA-E08803-115-1 and 119 show ad-finley and cl-finley being disabled and the cl-finley session being revoked. Records LA-E08803-116-1 and 120 show that no sessions were revoked because ad-finley didn't have an open session and the cl-finley session automatically revoked when the account was disabled. LA-E08803-118-1, 121-1, and 136-1 removed Finley from GA-ALL, GC-READ, and GA-SUP groups. The retest with the old session account (AT-E08803-123-1) was denied b/c the account is disabled. The sign-in attempt (LS-E08803-124-1) failed b/c the account is disabled.

## EV-E08803-024

- Slot E17 · M06 · revision 136
- Two distinct findings with raw-event references and correlation
- SHA-256 2750e7a7024a74e0d65eb5ba0c1b1fec798073ce5f8f5e63de1932d13dd9647a
- Caption: Records S004-S008 show the failed sign-in attempts. S009 shows the successful sign-in and has correlation ID C009 and is related to A002 that shows Casey being added to the Privileged Operators group.

## EV-E08803-025

- Slot E18 · M06 · revision 136
- Benign/ambiguous comparison
- SHA-256 2750e7a7024a74e0d65eb5ba0c1b1fec798073ce5f8f5e63de1932d13dd9647a
- Caption: The records show some suspicious activity with failed login attempts and then privilege escalation. But the records don't prove that there is malicious activity. More evidence is needed to come to a conclusion.

## EV-E08803-026

- Slot E19 · M06 · revision 138
- Completed case file / versioned export
- SHA-256 f7e42ebc0c4c38dfff17bfc2f16022c68a13f9f851fd802f8dc3f7d6468dffa6
- Caption: Sign-in records S004-S008 show the failed sign-in attempts. S009 shows the successful sign-in. A002 shows that right after sign-in happened, Casey's account was added to the Privilege Operators group, giving her higher permissions than she already had. No IT ticket was assigned to this change.
