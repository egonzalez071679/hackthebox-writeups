# Meow — Hack The Box Starting Point

> **Platform:** Hack The Box Labs — Starting Point  
> **Difficulty:** Very Easy  
> **Category:** Reconnaissance / Misconfiguration / Weak Credentials  
> **Completed:** 2026-05-31  
> **Machine Link:** https://app.hackthebox.com/starting-point

---

## Summary

First machine in the HTB Starting Point track. No exploit, no CVE — this one falls entirely through misconfiguration: Telnet running on port 23 with a blank root password. The lab walks through the foundational pentest workflow from scratch: connect to the environment, verify the target is up, scan for open services, identify the attack surface, test weak credentials, get a shell, retrieve the flag. The actual "attack" is pressing Enter at a password prompt. The point isn't complexity — it's learning to do the steps in order and understand why each one matters.

---

## Key Concepts Learned

- **Pwnbox** — HTB's browser-based Parrot OS instance; everything you need is pre-installed; no local VM setup required; clipboard utility in the bottom-right corner bridges your local clipboard into the VM
- **Target spawning** — Pwnbox (your attack machine) and the target machine are two separate things that both need to be initialized; easy to miss when you're new
- **`ping`** — ICMP check to confirm the target is actually reachable before you start scanning; skipping this wastes time troubleshooting scans that fail for network reasons
- **Nmap `-sV`** — service version detection; tells you not just that a port is open but what service and version is running on it
- **CPE (Common Platform Enumeration)** — standardized identifier for OSes and software; Nmap returns this string when it fingerprints the target OS
- **Telnet** — legacy remote management protocol on port 23; no encryption; everything including credentials goes over the wire in plaintext
- **Blank root password** — root account left with no password set; requires no credential cracking or privilege escalation; direct full access
- **Negative intelligence** — failed logins are not wasted attempts; they confirm which accounts are not viable and let you narrow your focus
- **`ls` / `cat`** — standard first moves after landing a shell; see what's there, read what matters

---

## Notes & Walkthrough

### Setting up in Pwnbox

I used Pwnbox for this one rather than OpenVPN. The setup flow on the HTB UI isn't totally obvious the first time through:

1. In the "Connect using Pwnbox" section, pick a server location close to you — I picked the closest available to minimize latency
2. Click **START PWNBOX** and wait for it to come online
3. Click **OPEN DESKTOP** — this opens the Pwnbox in a new browser tab
4. Back on the Meow lab page, click **SPAWN MACHINE** to initialize the target — this gives you the target IP

The thing that tripped me up initially: Pwnbox and the target are two completely separate instances. Pwnbox is your attack platform. The target is what you're attacking. Both need to be running before you can do anything.

Once the target IP appeared (mine was `10.129.251.148`), I copied it and used the **clipboard icon** in the bottom-right corner of the Pwnbox UI to paste it into the VM environment. Then opened the terminal from the top navigation bar.

### Step 1 — Verify connectivity

```bash
ping 10.129.251.148
```

```
PING 10.129.251.148 (10.129.251.148) 56(84) bytes of data.
64 bytes from 10.129.251.148: icmp_seq=1 ttl=63 time=84.2 ms
64 bytes from 10.129.251.148: icmp_seq=2 ttl=63 time=83.9 ms
```

Target is up and reachable. This takes two seconds and eliminates the possibility that a later scan failure is actually just a connectivity problem. Worth the habit.

### Step 2 — Service enumeration

```bash
sudo nmap -sV 10.129.251.148
```

```
Starting Nmap 7.92
Nmap scan report for 10.129.251.148
PORT   STATE SERVICE VERSION
23/tcp open  telnet  Linux telnetd
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

Port 23 open, running Telnet. That's the finding. Telnet has no encryption — it transmits everything in cleartext, including credentials. The CPE string confirms a Linux target. There's only one open port and it's one of the most historically abused services in legacy infrastructure. The attack surface here is basically one door.

### Step 3 — Testing credentials

```bash
telnet 10.129.251.148
```

```
Trying 10.129.251.148...
Connected to 10.129.251.148.
Escape character is '^]'.

 HTB Meow

Meow login: admin
Password:
Login incorrect

Meow login: administrator
Password:
Login incorrect
```

`admin` and `administrator` both reject. That's negative intelligence — now I know those aren't the right accounts (or at least not with blank passwords). Next obvious target is `root`.

### Step 4 — Root shell

```bash
Meow login: root
```

```
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-77-generic x86_64)

root@meow:~#
```

No password entered. Just pressed Enter. Root account has a blank password — the system accepted it and dropped me straight into a root shell. Full administrative access, no further steps needed.

### Step 5 — Flag retrieval

```bash
root@meow:~# ls
flag.txt  snap

root@meow:~# cat flag.txt
b40abdfe23665f766f9c61ecba8a4c19
```

Flag is sitting in the root home directory, which is where you land by default. `ls` to confirm it's there, `cat` to read it. Done.

---

## Tools Used

| Tool | Purpose |
|------|---------|
| **Pwnbox** | Browser-based attack machine; pre-configured Parrot OS with all tools included |
| **`ping`** | ICMP connectivity check before scanning |
| **`nmap -sV`** | Service and version detection on the target |
| **`telnet`** | Connect to port 23 and interact with the login prompt |
| **`ls` / `cat`** | List and read files after gaining shell access |

---

## Takeaways

- **Telnet sends everything in plaintext — including passwords.** Any host on the same network path can run Wireshark and read the credentials as they go by. There's no configuration that fixes this; it's how the protocol works. SSH is the replacement — same remote shell capability, everything encrypted. In a SOC context, Telnet traffic appearing on an internal network segment is worth investigating immediately. It shouldn't exist in modern infrastructure.
- **Blank root password = total compromise, no steps required.** Every other security control on this machine is irrelevant once the root account has no password. No credential cracking, no privilege escalation, no lateral movement — just type `root`, press Enter, and you're done. From a Blue Team perspective, an authentication event that succeeds on the first attempt with no password set, on a privileged account, over an unencrypted protocol, is as bad as it gets. The log entry for this would be in `/var/log/auth.log`.
- **Negative intelligence is real.** The `admin` and `administrator` failures weren't wasted — they told me something. In a real engagement with many accounts to test, tracking which ones reject blank or default credentials focuses your effort. You're not failing; you're narrowing.
- **The workflow matters more than the exploit.** This machine has no CVE. The "attack" is pressing Enter. But the workflow — connect, verify, scan, identify, test, exploit, retrieve — is identical to how a real engagement runs, just without the complexity. Getting the workflow right at this level is the whole point of Starting Point.
- **Root access means assume total host compromise in IR.** If an attacker lands a root shell on a host — however they got there — you treat the entire machine as fully compromised: every file readable, every config exposed, every credential stored on it in scope. In an IR investigation, that means expanding the blast radius to anything that machine had access to.

---

## References

- [Hack The Box — Starting Point](https://app.hackthebox.com/starting-point)
- [MITRE ATT&CK — Brute Force: Password Guessing (T1110.001)](https://attack.mitre.org/techniques/T1110/001/)
- [MITRE ATT&CK — Valid Accounts: Local Accounts (T1078.003)](https://attack.mitre.org/techniques/T1078/003/)
- [Nmap — Service Version Detection](https://nmap.org/book/man-version-detection.html)
