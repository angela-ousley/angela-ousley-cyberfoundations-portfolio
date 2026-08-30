# Week 7 Notes — Cloud Heights: The Guard Post

**Student Name:** Angela Ousley

**Week:** 7

## Firewall and Security Group

```text
The firewall and NSG have inbound and outbound rules that allow or deny network traffic. The rules contain a ranking (priority), info about the direction, source, destination, protocol, and port of the traffic. An action is assigned to each rule to deny or allow the traffic.
```

## Rule Anatomy

```text
(priority, direction, source, destination, protocol, port, action)
```

## First Match Wins

```text
"First match wins" means that the rule with the lowest priority number gets evaluated first. Once the traffic matches with the first rule, the rules after are not considered.
```

## Least Privilege

```text
Least privilege is when you are allowed just enough access to do your job, no more, no less.
```

## Testing and Evidence

```text
Testing is done to confirm the rules are functioning as intended. The evidence should show that the deny and allow rules both work. Proving the allow rule works shows connectivity, proving the deny rule works shows that unwanted traffic is blocked. Both outcomes need to have screenshots.
```

## Troubleshooting and Remediation

```text
When troubleshooting an allow rule that isn't working, you never want to change the rule to allow traffic to come to any port on your system because it'll open all the ports (doors) which increases the attack surface. It's tempting to do in order to see if there's an issue with the destination port, but it leaves your network exposed. Another troubleshooting method for rules that aren't working is to check their priority (rank). If you have "deny rule 300" and "allow rule 100", the allow rule will come first and the deny rule will not kick in. Always check the order of the rules 
```

## Questions I Still Have

```text
None at the moment.
```
