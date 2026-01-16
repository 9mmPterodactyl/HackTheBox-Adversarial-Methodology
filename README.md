# HackTheBox Adversarial Methodology

**by 9mmpterodactyl**

## Purpose and Scope

This repository documents my Hack The Box methodology, which is intentionally focused on adversarial system compromise rather than professional penetration testing. HTB machines are treated as hostile environments with no imposed scope beyond achieving initial access and escalating privileges to full system compromise.

The goal of this methodology is to develop and demonstrate an attacker’s mindset: identifying viable entry points, chaining weaknesses, and persisting through incomplete information until root access is achieved. Considerations such as reporting completeness, service stability, or business impact are intentionally deprioritized in favor of technical depth and exploitation efficiency.

This methodology is not intended to represent how I conduct scoped, client-authorized penetration tests. It exists to capture how I approach systems when operating from a purely offensive perspective, where the objective is total compromise rather than structured assessment.

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

Figure out the entry point and exploit it. This is where my skills as a web application hacker and bug bounty hunter come in, as i use my web-application-testing-methodology here to find critical 

vulnerabilities that can be used to gain access to the system. Our initial foothold into HTB systems usually is a busybox shell or a meterpreter shell, and we will need to stablize the shell for best performance.

All though, sometimes the initial foothold is using a critical vuln to dump info from backend tables that reveal credentials, sometimes giving a path to use SSH to login to the machine. 

**Common vectors** : web vulnerabilities (SQLi, command injection, file upload, LFI/RFI, template injection), service exploits for specific versions, default credentials, misconfigurations.

Get a reverse shell, stabilize it if necassary (example stablization script in python):
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
- WinPEAS for automated enumeration
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

Go back and enumerate more. Check what you missed. If still testing for Web Vulns and a way in, make sure you go through the OWASP Top 10 when trying to find that initial crack in the armor.

Most easy machines you can find a critical CVE affecting the machine with a nuclei scan, but medium, hard, and insane machines require a deep understanding of the OWASP Top 10 and a solid web testing methodology. 

Dont overlook downloadable source code from machines. this is gold for writing custom exploits. For example, in a machine I recently did, after donwloading the source code for website that allowed users to upload browser extensions, I can actually see in the source code how one can chain an SSRF vuln with a command injection vuln to gain access. 

Once a reverse shell is established, try the simple things - default credentials, obvious misconfigurations, checking sudo -l right after getting a shell.

---

## Tools

**Scanning & Enumeration:**
Nmap, Gobuster, ffuf, Nuclei, various subdomain/DNS tools

**Web Testing:**
Burp Suite, SQLmap, custom Python scripts, Metasploit

**Exploitation:**
Metasploit, Searchsploit/ExploitDB, custom exploits written by me or from GitHub and/or Claude, Netcat, python HTTP server,

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

Check the simple stuff first - software versions with known vulerabilities, default credentials, sudo -l immediately after shell access, obvious misconfigurations.

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
