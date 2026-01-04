# HackTheBox Labs 🎯

<div align="center">

![HackTheBox](https://img.shields.io/badge/HackTheBox-Player-9FEF00?style=for-the-badge&logo=hackthebox&logoColor=black)
![Machines Pwned](https://img.shields.io/badge/Machines%20Pwned-2-brightgreen?style=for-the-badge)
![Progress](https://img.shields.io/badge/Progress-Active-blue?style=for-the-badge)

**CTF Writeups, Penetration Testing Notes, and Lab Documentation**

[Profile](https://app.hackthebox.com/users/2999443) • [TryHackMe](https://tryhackme.com/p/salim.hadda) • [GitHub Profile](https://github.com/samli-neo)

</div>

---

## 📖 About This Repository

This repository contains detailed writeups, methodologies, and documentation for HackTheBox machines I've completed. Each writeup includes:

- 🎯 **Complete methodology** from reconnaissance to exploitation
- 💻 **Command-line examples** with explanations
- 🛠️ **Tools and techniques** used during exploitation
- 🔒 **Security insights** and remediation guidance
- 📚 **Learning resources** for further study

**Note:** Flags are intentionally redacted to encourage learning over copying. Focus on understanding the methodology!

---

## 🏆 Statistics

<div align="center">

| Metric | Count |
|--------|-------|
| **Machines Pwned** | 2 |
| **Starting Point** | 2/24 |
| **Tier 0** | 2/8 |
| **Tier 1** | 0/8 |
| **Tier 2** | 0/8 |
| **Easy Machines** | 0 |
| **Medium Machines** | 0 |
| **Hard Machines** | 0 |
| **Insane Machines** | 0 |

</div>

---

## 🎮 Starting Point Progress

### Tier 0 (Very Easy) - 2/8 Completed

| # | Machine | OS | Difficulty | Status | Writeup | Topics |
|---|---------|----|-----------:|:------:|:-------:|--------|
| 1 | **Meow** | Linux | ⭐ Very Easy | ✅ | [📝](https://github.com/samli-neo/hackthebox-labs/tree/main/Starting-Point/Tier-0/Meow) | Telnet, Weak Credentials |
| 2 | **Fawn** | Linux | ⭐ Very Easy | ✅ | [📝](https://github.com/samli-neo/hackthebox-labs/tree/main/Starting-Point/Tier-0/Fawn) | FTP, Anonymous Login |
| 3 | **Dancing** | Windows | ⭐ Very Easy | 🔒 | - | SMB, Network Shares |
| 4 | **Redeemer** | Linux | ⭐ Very Easy | 🔒 | - | Redis, NoSQL |
| 5 | **Explosion** | Windows | ⭐ Very Easy | 🔒 | - | RDP, Remote Desktop |
| 6 | **Preignition** | Linux | ⭐ Very Easy | 🔒 | - | Web, Directory Enumeration |
| 7 | **Mongod** | Linux | ⭐ Very Easy | 🔒 | - | MongoDB, NoSQL |
| 8 | **Synced** | Linux | ⭐ Very Easy | 🔒 | - | Rsync, File Sync |

### Tier 1 (Easy) - 0/8 Completed

| # | Machine | OS | Difficulty | Status | Writeup | Topics |
|---|---------|----|-----------:|:------:|:-------:|--------|
| 1 | **Appointment** | Linux | ⭐⭐ Easy | 🔒 | - | SQL Injection |
| 2 | **Sequel** | Linux | ⭐⭐ Easy | 🔒 | - | MySQL, Databases |
| 3 | **Crocodile** | Linux | ⭐⭐ Easy | 🔒 | - | FTP, Web Exploitation |
| 4 | **Responder** | Windows | ⭐⭐ Easy | 🔒 | - | NTLM, Windows Auth |
| 5 | **Three** | Linux | ⭐⭐ Easy | 🔒 | - | AWS S3, Cloud |
| 6 | **Ignition** | Linux | ⭐⭐ Easy | 🔒 | - | Web, CMS |
| 7 | **Bike** | Linux | ⭐⭐ Easy | 🔒 | - | SSTI, Template Injection |
| 8 | **Funnel** | Linux | ⭐⭐ Easy | 🔒 | - | PostgreSQL, Tunneling |

### Tier 2 (Easy) - 0/8 Completed

Coming soon...

---

## 📝 Featured Writeups

### ✅ Meow - Telnet Weak Credentials

**Completed:** 04 Jan 2026 | **Player:** #533326

```bash
# Key Commands
nmap -sV <target_ip>
telnet <target_ip>
> root
cat flag.txt
```

**Learning Outcomes:**
- VPN setup and connectivity
- Basic network reconnaissance with nmap
- Telnet protocol fundamentals
- Default/weak credential exploitation
- Linux file system navigation

[📖 Read Full Writeup →](./Starting-Point/Tier-0/Meow)

---

### ✅ Fawn - FTP Anonymous Login

**Completed:** 04 Jan 2026 | **Player:** #437600

```bash
# Key Commands
nmap -sV -sC -p 21 <target_ip>
ftp <target_ip>
> anonymous
> ls
> get flag.txt
cat flag.txt
```

**Learning Outcomes:**
- FTP protocol fundamentals (port 21)
- Anonymous authentication vulnerabilities
- vsftpd service enumeration
- File transfer operations
- Cleartext protocol security risks
- SFTP vs FTP vs FTPS comparison

[📖 Read Full Writeup →](./Starting-Point/Tier-0/Fawn)

---

## 🛠️ Tools & Techniques

### Reconnaissance
- **nmap** - Network scanning and service enumeration
- **ping** - Connectivity testing (ICMP)
- **netcat (nc)** - Network utility for reading/writing

### Enumeration
- **nmap scripts** (`-sC`, `-sV`) - Service version detection
- **enum4linux** - SMB enumeration
- **nikto** - Web server scanning

### Exploitation
- **telnet** - Remote access protocol
- **ftp** - File Transfer Protocol client
- **smbclient** - SMB/CIFS client
- **redis-cli** - Redis database client

### Post-Exploitation
- **cat**, **ls**, **pwd** - File system navigation
- **find** - Search for files
- **grep** - Pattern matching

---

## 📚 Learning Resources

### Official Documentation
- [HackTheBox Academy](https://academy.hackthebox.com/) - Free training modules
- [Starting Point Guide](https://help.hackthebox.com/en/articles/5185687-introduction-to-lab-access)
- [HTB University](https://www.hackthebox.com/universities)

### Recommended Reading
- **Networking:**
  - TCP/IP Illustrated (Stevens)
  - CompTIA Network+ Study Guide
- **Penetration Testing:**
  - The Hacker Playbook 3 (Peter Kim)
  - RTFM: Red Team Field Manual (Ben Clark)
- **Linux:**
  - Linux Command Line and Shell Scripting Bible
  - How Linux Works (Brian Ward)

### Practice Platforms
- [TryHackMe](https://tryhackme.com/) - Guided learning paths
- [VulnHub](https://www.vulnhub.com/) - Downloadable VMs
- [PentesterLab](https://pentesterlab.com/) - Web security focus

---

## 🎯 Current Goals (2026)

- ✅ Complete Starting Point Tier 0 (2/8)
- ⏳ Complete Starting Point Tier 1 (0/8)
- 🎯 Complete Starting Point Tier 2 (0/8)
- 🎯 Pwn 10+ HackTheBox machines
- 🎯 Document all methodologies with detailed writeups
- 🎯 Earn "Script Kiddie" rank

---

## 📊 Progress Tracker

```
Starting Point Progress: ██░░░░░░░░ 8% (2/24)
Tier 0: ██░░░░░░ 25% (2/8)
Tier 1: ░░░░░░░░  0% (0/8)
Tier 2: ░░░░░░░░  0% (0/8)
```

**Last Updated:** 04 January 2026

---

## 🔐 Ethical Hacking Guidelines

All activities documented in this repository follow ethical hacking principles:

✅ **Permission** - Only attack systems you own or have explicit authorization to test
✅ **Responsible Disclosure** - Report vulnerabilities through proper channels
✅ **Education** - Focus on learning and skill development
✅ **Legal Compliance** - Adhere to all applicable laws and regulations

**⚠️ Disclaimer:** The techniques and tools discussed are for educational purposes only. Unauthorized access to computer systems is illegal. Always obtain proper authorization before testing.

---

## 🤝 Connect

- **HackTheBox:** [SalimHadda](https://app.hackthebox.com/users/2999443)
- **TryHackMe:** [salim.hadda](https://tryhackme.com/p/salim.hadda)
- **LinkedIn:** [Salim Hadda](https://linkedin.com/in/haddasalim)
- **GitHub:** [@samli-neo](https://github.com/samli-neo)

---

## 📄 License

This repository is for educational purposes. All writeups are original work based on publicly available HackTheBox machines.

---

<div align="center">

**🔒 Keep Learning. Stay Curious. Hack Responsibly.**

![Visitor Count](https://komarev.com/ghpvc/?username=samli-neo-htb&color=9FEF00&style=for-the-badge)

</div>
