# Week 5 Notes — The Grid: Addresses, Names, Ports, and Diagnostics

**Student Name:** Angela Ousley

**Date Completed:** Aug 18, 2026

Summarize this week's key concepts in your own words — not copy-pasted definitions.

## Key Concepts This Week

- IP addresses — the dotted-quad number every device on a network needs (`10.20.5.42` on The Grid)
- The subnet mask — the answer to "which addresses are my neighbours?" (`/24` = `255.255.255.0`)
- The default gateway — the door out of your neighbourhood (`10.20.5.1` on The Grid)
- Private vs public addresses — `10.x`, `172.16–31.x`, and `192.168.x` are *inside* addresses
- DNS — the Grid's Directory Board: a name goes in, an IP address comes out
- NXDOMAIN vs a host that resolves but is down — two different failures with two different causes
- DHCP — the Address Office: leases, why addresses change, why a laptop "just works" on a new network
- Ports — the numbered doors on a building: 22 SSH, 53 DNS, 80 HTTP, 443 HTTPS, 3389 RDP, 25 SMTP
- TCP vs UDP — a confirmed conversation vs a shout across the room
- The TCP handshake — SYN → SYN-ACK → ACK (packets 7, 8 and 9 in Lab 03)
- The diagnostic toolkit — `ping` (is it alive?), `traceroute` (where does it stop?), `dig` (what number is behind that name?)
- **THE LADDER RULE** — check yourself → check your gateway → check the target by NAME → check the target by IP → trace the path. *Work outward, one rung at a time, and let the evidence pick the culprit.*

## My Command Table

You learned the same five jobs twice this week — once in bash, once in PowerShell. Fill the pairs in from memory if you can, and check them afterwards. This table is worth keeping.

The bash command and its PowerShell equivalent for each job — show my own address, show my default gateway, test reachability, trace the path, look up a name:

```
Bash           PowerShell
ip addr        ipconfig
ip route       ipconfig
ping           Test-Connection
traceroute     tracert
dig            Resolve-DnsName
```

## In My Own Words

Your machine has three numbers: an address, a subnet mask, and a default gateway. Explain what each one is for, the way you'd explain it to someone who has never heard those words.

```
An IP address is like your home address. The way that mail is sent to your house using your home address is how data is sent to your computer using your IP address. A subnet mask lets you know which part of the address is your local network (neighborhood) and which part of the address is the host (individual machine). A default gateway is like a big gate that opens so you can leave your neighborhood and travel to other areas, the default gateway allows data to travel outside of your local network to other computers outside of your network.
```

What does DNS actually do? Include the difference between a name that comes back "Name or service not known" (NXDOMAIN) and a name that resolves perfectly well to a host that never answers.

```
The DNS (domain name system) turns a website name into an IP address. A name that comes back "Name or service not known" means that the domain name is decommissioned or not registered. A name that resolves perfectly to a host that never answers means that the machine is powered off or has crashed.
```

An IP address gets your traffic to the right building. What does a port number add to that, and why would a defender care how many doors are open?

```
A port number gets you to the right door of the building. A defender cares about how many doors are open because they need to know what to protect. Every open door adds to the attack surface.
```

Write out THE LADDER RULE — all five rungs, in order — and say why running them in that order matters more than running them fast.

```
1. Check yourself [ip addr]. Confirm your address is working.
2. Check your gateway [ping]. Make sure the data can leave the local network.
3. Check the name [ping]. Make sure the website name resolves to an IP address.
4. Check the IP address [ping]. Make sure the IP address answers.
5. Check the route [traceroute]. See how far the packet goes.
Running these in order matters more than running them fast because you always want to make sure your local network isn't the issue first. If you start checking outside of your network first, you may blame DNS or the server you're trying to connect with, but it could be your own gateway not opening that causing the real issue.
```

What is DHCP, and why does your laptop get an address automatically on a network it has never joined before, while a server like `grid-dns` keeps the same address permanently?

```
DHCP (dynamic host configuration protocol) is a tool that automatically gives an IP address to a device joining the network. Your laptop gets an address automatically on a network it has never joined before because the DHCP gives a lease to your device for a temporary address that's available to use.  A server uses a static address making it permanent. A static address ensures easy access so computers can always find the server on a network without the address changing.
```

---

## Submission Checklist

- [x] I summarized each concept in my own words, not copied definitions

- [x] I completed the bash-to-PowerShell command table

- [x] I answered all five "In My Own Words" prompts

- [x] This file is committed to my portfolio repo at `week-05/notes.md`

---

*CyberVisionaries Institute — Cyber Foundations, Tier I*
