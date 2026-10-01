# Week 10 — Lab 2: Prioritize Risks and Recommend Controls

Learner: Angela Ousley
Case: Cloud Heights Family Clinic — Risk & Threat Investigation
Scenario date: Friday 13 March 2026, 09:00 (clinic local time)
Report generated: 2026-10-01T16:39:46.905Z
Study mode: Guided (hints available)

Completion checklist: all required work for Lab 2 is present.

## Risk ratings

| ID | Asset | Likelihood | Why | Impact | Why | Score (L x I) | Classroom band |
| --- | --- | --- | --- | --- | --- | --- | --- |
| SC-01 | Appointment scheduling account | 3 | It's a high likelihood because staff receives a phishing email once a week, that's routine exposure. If there are any controls or reliable protection, it is unknown. | 3 | It's a hight impact  because an authorized person now has access to patient appointments, so patient data is exposed. Operations could stop if the attacker blocks access to the scheduling account and the backup data can't be restored. If it can be restored or not is currently unknown. | 9 | High (6–9) |
| SC-02 | Patient records application | 3 | I give an overall likelihood score of 3 because of the shared login for the front desk. Routine exposure of using the same login for 4 different employees. No protection identified for non\-repudiation. I give a score of 1 for someone gaining access to the front desk password because the controls are unknown. I don't know if they keep the office door closed when they leave, if the door has a code for entry, etc. I give the former employee gaining access to the system outside of the office a score of 1 because it's unknown whether that can happen or not. | 3 | I give the impact a 3 because if an employee mistakenly deletes all the records for the patients being seen today, operations would come to a halt. Can the backup restore the patient records? We don't know, so consider the data lost. Recovery is uncertain. | 9 | High (6–9) |
| SC-03 | Staff laptops | 3 | I give a high likelihood overall due to security patches being pushed back for 90 days on a couple of computers. That's routine exposure to an opening or weakness in the software. I don't know how often the staff leaves the workspace door open or how often they leave their laptops unlocked and signed in. | 3 | The impact is high because a threat actor who decides to exploit the hole in the security software will have access to staff email and patient records. | 9 | High (6–9) |
| SC-04 | Public information website | 2 | Likelihood is medium because it has happened before, but it has partial protection because the public website isn't connected to the rest of the system \(records, appts, laptop, or backup\). | 1 | Low impact because the public website doesn't directly affect operations. Operations can continue if the public website is down, no sensitive data is leaked. The public website certificate being expired could lead to loss of trust with clients and turn prospective clients away. | 2 | Low (1–2) |
| SC-05 | Backup archive | 3 | High likelihood due to staff never testing to see if restoring the data works as intended. There's also only one backup and it's kept at the clinic instead of an offsite location, so there isn't reliable protection. | 3 | High impact because if there's ever an instance where the backup is needed, it may or may not work. Since the original data and the backup are kept in the same location, operations would stop if computers and that backup were stolen from the clinic. | 9 | High (6–9) |

Bands (1–2 low, 3–4 medium, 6–9 high) are a classroom teaching aid, not a compliance standard.

## Priority risks
- SC-02 — Patient records application
- SC-03 — Staff laptops

**Why these:** I choose these two first because patient records having the chance of being exposed is a huge risk. The clinic may not be able to recover from that. The staff laptops not having proper security updates leaves a vulnerability that can expose all the data from the clinic.

## Recommended controls
### SC-02
- **Control:** They could stop keeping the 'frontdesk' password written down and require staff to remember it. Change the password more often so only current staff members have access to it. The best control would be to give separate accounts to the front desk staff.
- **How it helps:** Having the staff remember the password instead of writing it down helps keep the password more secure, less exposure. Changing the password more often helps prevent unauthorized access from happening or continuing if the password is exposed. Giving each receptionist their own login allows non-repudiation and accountability.
- **Risk remaining afterwards:** The remaining risk of using one login for 4 staff members is not knowing who did what.

### SC-03
- **Control:** They could setup the computers to do a forced security update after postponing for more than a certain number of days. They could set the records application to automatically sign out a user after a certain amount of time of inactivity. Set the computer to automatically lock after a certain time of inactivity. Make it a habit of closing the workspace door when leaving out.
- **How it helps:** Automating security updates keeps the holes/vulnerabilities closed, making it less likely for attackers to hack the system. Setting the application and computer to automatically sign out and lock after a certain period of time reduces the chances of an unauthorized user gaining access to the system. Closing the door to the office reduces the chances of data exposure and theft of devices.
- **Risk remaining afterwards:** There's always a chance of a hacker figuring out a way to enter a system.

## Owner briefing
Word count: 271 (guide: 100–150)

The clinic has some high risk issues, but there is opportunity for prevention. Staff receives at least one phishing email a week. Blocking unknown email addresses could help with that. Keeping passwords written down is extremely risky behavior. Please require staff to stop writing down passwords. Once a staff member leaves that knows the password, please change the password immediately. We don't know if it's possible for them to access the system outside of the office, so it's better to play it safe and change the password. Having one login for 4 different staff members isn't good for auditing purposes, you should always be able to show which staff member is doing what in the system. Keep all security software updates/patches current. Do not allow staff to postpone updates more than a couple of days; once vulnerabilities are public it doesn't take experienced hackers long to figure out a way to exploit them. Make sure staff are locking their computers when stepping away and closing doors behind them when leaving the office. These are simple ways to prevent unauthorized access to devices and data. Keep the certificate up to date for the public website. An expired certificate can scare away new potential clients and cause current patients to lose trust with the clinic. Please store the backup in a location outside of the office if possible, having the backup in the same location the data is collected isn't a good idea. Test to make sure the data on the backup can be properly restored, that way you know  backups are working as intended and the information will be available when needed.
