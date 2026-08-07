# Week 4 Notes — Permissions, Searching, and Virtual Machines

**Student Name:** Angela Ousley

**Date Completed:** August 7, 2026

Summarize this week's key concepts in your own words — not copy-pasted definitions.

## Key Concepts This Week

- File permissions: read/write/execute × owner/group/other, and reading `ls -l`
- Changing permissions with `chmod` (symbolic and numeric) — and THE GATEKEEPER'S RULE
- Windows ACLs, read with `Get-Acl`/`icacls`
- Wildcards (`*`, `?`, `[ ]`) and searching inside files with `grep`/`Select-String`
- Virtual machines: host vs. guest, the hypervisor, Type 1 vs. Type 2, isolation
- The VM lifecycle: create, start, stop (deallocate), snapshot, delete — and what each costs
- Golden snapshots — how your Weeks 6–12 lab machines are made

## In My Own Words

**Decode `-rw-r-----` audience by audience: who can do what to this file?**

```
The owner can read and write. The group can read. The others can't do anything with the file, no reading, writing, or executing.
```

**What is a hypervisor, and what are its two jobs?**

```
A hypervisor is software that is on the hardware or OS of the host machine. It creates the virtual machine. Its two jobs are to isolate each VM and divide the host's resources among the guest machines.
```

**A stopped VM still costs a little money. What is it paying for, and what's the only way to reach a true zero?**

```
A stopped VM still charges for storage space on the disk. The only way to reach a true zero is to delete the virtual machine.
```

---

## Submission Checklist

- [x] I summarized each concept in my own words, not copied definitions

- [x] I answered all three "In My Own Words" prompts

- [ ] This file is committed to my portfolio repo at `week-04/notes.md`
