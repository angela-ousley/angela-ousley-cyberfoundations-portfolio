# Week 10 — Lab 1: Investigate What Needs Protection

Learner: Angela Ousley
Case: Cloud Heights Family Clinic — Risk & Threat Investigation
Scenario date: Friday 13 March 2026, 09:00 (clinic local time)
Report generated: 2026-09-30T03:59:11.306Z
Study mode: Guided (hints available)

Completion checklist: all required work for Lab 1 is present.

## Evidence added to my findings
- EV-REC-02 — Sender address comparison card
- EV-REC-OFF-01 — Records application account list
- EV-WKS-03 — Desk photo — signed-in laptop
- EV-WKS-01 — Update status report (IT contractor, 13 March 2026)
- EV-WEB-02 — Renewal process note
- EV-REC-OFF-03 — Practice manager statement — leavers
- EV-BAK-03 — Backup drive photo
- EV-BAK-01 — Backup job history
- EV-BAK-02 — Restore testing statement
- EV-REC-03 — Reception desk log note
- EV-REC-OFF-02 — Records access log extract
- EV-WEB-03 — What the public website actually holds

## My investigation notebook
Reception
User credential and access to the schedule app need to be protected.
I noticed that social engineering is being used in EV-REC-01. The staff statement mentioned they get emails like this about weekly, is this being reported to IT? EV-REC-02: The message came from 'clinic-support.example', is IT screening the email senders to make sure this type of email doesn't make it to the reception emails? EV-REC-03: They only use email and password to sign in, so no MFA?

Records Office
Patient information and staff login credentials need to be protected.
A password written down in the desk drawer is risky. If someone who isn't authorized can access the desk drawer, they now have the password. The same login for 4 people doesn't allow you to see who did what. Can former employees access records if they remember the login information?

Staff Workspace
The computer itself, the applications and data on the computer needs protection.
Allowing a computer to go w/o security updates for 90 days is a vulnerability. After a certain amount of time, a mandatory update should be initiated automatically during after work hours. Leaving a laptop unlocked is poor office etiquette. Someone can make a change on your computer and now you don't know who did what. Staff email and patient records are at risk of being accessed on an open computer that's left unattended and open.

Website Station
The security certificate needs protection so it needs to stay current.
No reminders and no one assigned to updating the certificate allows it to expire. This isn't good because a visitor to the website has to alert the company that the cert is expired. An expired cert message can scare away visitors, cause them to lose access to website info, and also hurt their trust within the clinic. That leads to loss of business for the clinic. The website is also open to attacks because the connection isn't secure.

Backup Room
The backup data and the physical drive need to be protected.
The data might be corrupted based on the error "6 job entries reading 'completed with errors'". There is only one backup and it's stored in the same location where the data is collected. Data has never been restored to see if the backups are working properly. The drive is easily accessible by anyone, including visitors.

## Risk scenarios

| ID | Asset | Evidence | Threat / event | Vulnerability | Consequence | CIA | Unknown / question |
| --- | --- | --- | --- | --- | --- | --- | --- |
| SC-01 | Appointment scheduling account | EV\-REC\-02 \(Sender address comparison card\); EV\-REC\-03 \(Reception desk log note\); EV\-REC\-01 \(Email received at the reception mailbox\) | If an employee falls for the phishing email, staff credentials are now leaked to an outside party. | Emails from an unverified third party are currently allowed to to reach the staff email. This allows attackers to directly contact staff with socially engineered emails. | The credentials can now be used by an attacker to alter or cancel appointments. They might even install ransomware to block access to the scheduling app until they receive a payment. | Confidentiality, Integrity, Availability | Did anyone at the clinic reply to the phishing email or enter a password on the link in the email?<br>Are these messages being reported to the IT contractor?<br>Are emails from unverified senders being blocked? |
| SC-02 | Patient records application | EV\-REC\-OFF\-01 \(Records application account list\); EV\-REC\-OFF\-02 \(Records access log extract\); EV\-REC\-OFF\-03 \(Practice manager statement — leavers\) | An unauthorized individual can gain access to the 'front desk' password and gain access to patient records. Someone can make negative changes to patient records, but the shared login doesn't allow accountability to a specific employee. If a former employee can access the records system, they can  read/edit patient records and they are no longer authorized to do so. | The password is currently unprotected because it's written down. Four employees all use the same login so there's no non\-repudiation. Keeping the same password for the account once an employee that knows the password leaves is risky. | An unauthorized individual might leak/sell patient data. They could block staff from accessing patient records. A current/former employee or any unauthorized user with access to the login credentials can make changes to records and you'll never know who it was, that will look negative in a security audit. | Confidentiality, Integrity | The evidence doesn't show if a former employee does have access to the 'front desk', but it's still a possibility. |
| SC-03 | Staff laptops | EV\-WKS\-01 \(Update status report \(IT contractor, 13 March 2026\)\); EV\-WKS\-03 \(Desk photo — signed\-in laptop\) | An attacker can exploit a known vulnerability that hasn't been patched/updated yet; the longer it takes for a known vulnerability to get fixed, the more time an attacker can figure out how to exploit the hole in the security software. Leaving the door to the Staff Workspace allows anyone to walk in, unauthorized employees or patients. Leaving the laptop unlocked and signed into the records application allows an unauthorized individual to view \(maybe even edit\) patient records. | Security updates getting postponed for 90 days. Leaving the door to the Staff Workspace open. Not locking the laptop when stepping away from the desk. | A threat actor gains access to patient records due to the security update being postponed. The clinic now has a HIPAA violation. Trust is lost in the clinic. The door to the office was left open, someone stole the keyboard. Now the office is down a computer and can't access what's needed to run the office. A random guy walks into the office since the door is open and sees the computer is unlocked and signed in. He looks through the records and finds the medical history for his ex\-wife. He edits the record to make himself her emergency contact. Another HIPAA violation for the clinic. | Confidentiality, Integrity, Availability | N/A |
| SC-04 | Public information website | EV\-WEB\-02 \(Renewal process note\); EV\-WEB\-03 \(What the public website actually holds\) | The certificate expires and visitors can no longer access the website or are scared to access the site because the connection isn't secure and their browser will alert them not to enter. | The certificate for the clinic's website will expire an no one in IT will know to have it reissued. | Possible patients look for another clinic because the Cloud Heights Family Clinic website isn't safe to visit. The clinic is losing business. Patient trust is lost. | Availability | N/A |
| SC-05 | Backup archive | EV\-BAK\-01 \(Backup job history\); EV\-BAK\-02 \(Restore testing statement\); EV\-BAK\-03 \(Backup drive photo\) | A fire happens at the clinic. The backup device was in the clinic so they don't have any data that can be restored. Another scenario is they have the backup, but since it was never restored in the past, they just find out that the data for the entire year is corrupted and missing information. The storage device sits on the front desk at the reception section. The receptionist goes to the bathroom and a visitor walks in. The visitor steals the backup storage. | The backup is stored in the same location where the original data is held. The backed up data has never been deployed. The storage device is easily accessible. | If the backup gets destroyed, the clinic will lose everything they have. The data is gone and so are the patients and their trust in the company. Records that have been restored, but are invalid due to corruption doesn't retain patient trust, the clinic will lose business. A backup device being stolen can lead to patient info being leaked if the disk isn't encrypted. The device being stolen is a HIPAA violation. | Confidentiality, Integrity, Availability | Is the backup storage disk encrypted? |

## Email analysis
1. The urgency of the email. Urgency is a tactic used by attackers to create panic and cause someone to rush to do something.
2. The threat of the account being locked is a red flag. This creates fear of consequences by the recipient.
3. The email came from an unfamiliar address \(clinic-support.example\) instead of the email is usually comes from.

**Safe response / reporting step:** I wouldn't click on anything in the email, especially active links or attachments. I would report the email to IT or the Cybersecurity team immediately. I'd alert my coworkers not to click on that link until IT confirms it's safe.

**Suspicious vs proven:** The message proves that someone is trying to impersonate the clinic's real scheduling vendor to access credentials. I'd need to research what I can find about the email address that sent the phishing email. I'd open the link in the email in a sandbox environment to see where it leads to.
