# Hands-On Lab — Metasploitable 2 FTP Enumeration
## Objective
Apply the Nmap knowledge learned previously to a deliberately vulnerable
Metasploitable 2 virtual machine in an isolated VirtualBox lab.
The objectives were to:
- Discover open ports
- Identify services
- Identify service versions
- Enumerate the FTP service
- Validate a security finding manually
## Lab Environment
### Ubuntu — Attacker
- IP Address: `192.168.56.101`
- Network: VirtualBox Host-Only
### Metasploitable 2 — Target
- IP Address: `192.168.56.102`
- Network: VirtualBox Host-Only
The target was kept on the isolated Host-Only network.
## 1. Connectivity Test
Before scanning, I confirmed that Ubuntu could communicate with the
Metasploitable target.
Command:
```bash
ping -c 4 192.168.56.102
-Result: 4 packets transmitted, 4 received, 0% packet loss

2. Initial Nmap Scan
Command:
```bash
nmap 192.168.56.102
Nmap identified multiple open TCP ports, including:
21/tcp — FTP
22/tcp — SSH
23/tcp — Telnet
25/tcp — SMTP
53/tcp — DNS
80/tcp — HTTP
139/tcp — NetBIOS
445/tcp — SMB
3306/tcp — MySQL
3. Service and Version Detection

Command:
```bash
nmap -sV 192.168.56.102

Nmap identified the FTP service as:

21/tcp open ftp vsftpd 2.3.4

4. FTP Enumeration

I performed a more focused scan against port 21:

```bash
nmap -sC -sV -p 21 192.168.56.102

Nmap reported:

Anonymous FTP login allowed (FTP code 230)

It also reported that the FTP control and data connections were
unencrypted.

5. Manual Validation

I manually tested the anonymous FTP finding using the FTP client.

Command:

```bash

ftp 192.168.56.102

I connected to the FTP service and attempted anonymous authentication.

Result:

Login successful
Remote system type is UNIX
Using binary mode

This confirmed that anonymous FTP authentication was enabled.

6. Directory Enumeration

After authentication, I used:

ls

The FTP server returned:

229 Entering Extended Passive Mode
150 Here comes the directory listing.
226 Directory send OK.

No files or directories were displayed.

Finding:
Anonymous FTP Authentication Enabled

Target: 192.168.56.102

Port: 21/tcp

Service: FTP

Version: vsftpd 2.3.4

Status: Confirmed

Potential Impact:

Anonymous users can authenticate to the FTP service without a normal
user account.The actual impact depends on the permissions and resources available
to the anonymous account.During this test, no files were visible in the initial directory.

Recommendation:

Disable anonymous FTP access unless there is a documented requirement
for it.If anonymous access is required, restrict permissions and ensure that
only intended resources are exposed.

Key Lessons Learned
.An open port does not automatically mean that the service is vulnerable.
.Nmap can identify open ports, services and software versions.
.Nmap scripts can identify potentially insecure configurations.
.Scanner findings should be manually validated.
.Evidence should be collected before assigning severity.
.The impact of a finding depends on the actual access and permissions
.available.
.Testing should remain within an authorised and isolated environment.

Tools Used: 
-VirtualBox
-Ubuntu
-Metasploitable 2
-Nmap
-FTP client
5432/tcp — PostgreSQL
5900/tcp — VNC
8180/tcp — HTTP
