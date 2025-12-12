# HackTheBox Penetration Testing Methodology

**by 9mmpterodactyl**

How I approach compromising HTB machines. Enumerate, exploit, escalate.

**Current Stats:**
- Global Rank: #707 (Top 0.03%)
- Season 9 Rank: Ruby #1681
- Machines Solved: 21 (55 flags captured)

---

## The Process

### Reconnaissance

Scan everything, find what's running, identify versions.

```bash
nmap -sV -sC -p- -Pn --min-rate 5000 <target>
```

Full port scan, service enumeration, default scripts. No rate limiting concerns here - HTB machines can handle it.

**Subdomain enumeration** if there's DNS involved. **Directory enumeration** on any web services - ffuf or gobuster.

Find version numbers. Check for known CVEs. Look for anything unusual.

### Initial Access

Figure out the entry point and exploit it.

Common vectors: web vulnerabilities (SQLi, command injection, file upload, LFI/RFI, template injection), service exploits for specific versions, default credentials, misconfigurations.

Get a reverse shell, stabilize it:
```bash
python -c 'import pty; pty.spawn("/bin/bash")'
```

### Privilege Escalation

Once in as low-privilege user, enumerate the system.

**Linux:**
- LinPEAS for automated enumeration
- Check SUID binaries, sudo privileges, cron jobs
- Search for credentials in config files, bash history, databases

**Windows:**
- WinPEAS for enumeration
- Token impersonation (very common)
- Service misconfigurations, scheduled tasks
- Credential hunting in registry and config files

**Active Directory:**
- BloodHound for attack path mapping
- Kerberoasting, AS-REP roasting
- Credential dumping with Mimikatz
- Look for weak ACLs, delegation issues
- Pass-the-hash when needed

### When Stuck

Go back and enumerate more. Check what you missed. Try the simple things - default credentials, obvious misconfigurations, checking sudo -l right after getting a shell.

Most solutions are simpler than they seem at first.

---

## Tools

**Scanning & Enumeration:**
Nmap, Gobuster, ffuf, Nuclei, various subdomain/DNS tools

**Web Testing:**
Burp Suite, SQLmap, custom Python scripts

**Exploitation:**
Metasploit, Searchsploit/ExploitDB, custom exploits from GitHub, Netcat

**Post-Exploitation:**
LinPEAS, WinPEAS, BloodHound, Mimikatz, enumeration scripts

**Credential Attacks:**
Hashcat, John the Ripper, Hydra, Kerbrute

**Custom Tools:**
Python scripts for specific vulnerabilities, automation, niche exploits that don't have existing tools. Built or modified as needed.

---

## Difficulty Breakdown

**Easy:** Straightforward vulnerabilities, clear exploitation paths

**Medium:** Chain multiple vulnerabilities, deeper enumeration required

**Hard:** Obscure techniques, multiple privilege escalation steps, sometimes custom exploits

**Insane:** All of the above plus additional complexity, deep understanding of specific technologies

---

## What I've Learned

Enumeration is everything. When stuck, enumerate more.

Version numbers matter - specific versions have known exploits.

Check the simple stuff first - default credentials, sudo -l immediately after shell access, obvious misconfigurations.

Take notes. Easy to forget what you already tried.

Stabilize shells immediately.

---

## Resources

- HackTricks (comprehensive techniques)
- PayloadsAllTheThings (exploit payloads)
- RevShells (reverse shell generator)
- ExploitDB (public exploits)

---

Detailed machine writeups with problem-solving process: My-HTB-Writeups

---

**Note:** For authorized testing only. HTB and similar platforms where you have explicit permission.
