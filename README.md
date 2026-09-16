# Cybersecurity Learning Portfolio

I am **Olumide Oladeinde**, a CompTIA PenTest+ certified professional developing practical skills in penetration testing, network security, vulnerability assessment, and technical research.

This repository documents my learning journey through concise study notes and hands-on exercises performed only in authorised, isolated lab environments.

## Skills demonstrated

- Network and service enumeration with Nmap
- TCP/IP, ports, and common network services
- Manual validation of scanner findings
- FTP and HTTP/WebDAV enumeration
- Evidence-based security findings and remediation guidance
- Clear technical documentation

## Hands-on labs

| Lab | Focus | Key outcome |
|---|---|---|
| [Metasploitable 2 FTP Enumeration](Nmap/metasploitable-ftp-enumeration.md) | Nmap, FTP, manual validation | Confirmed anonymous FTP authentication on vsftpd 2.3.4 |
| [Metasploitable 2 WebDAV Enumeration](Labs/02-metasploitable-webdav-enumeration.md) | HTTP headers, OPTIONS, PROPFIND | Confirmed WebDAV exposure and enumerated supported methods |

## Learning notes

- [Networking fundamentals](Networking/networking-notes.md)
- [Nmap basics](Nmap/nmap-notes.md)

## Lab environment

- Oracle VirtualBox
- Ubuntu attacker VM: `192.168.56.101`
- Metasploitable 2 target VM: `192.168.56.102`
- Isolated host-only network: `192.168.56.0/24`

## Methodology

Each lab follows a repeatable workflow:

1. Define the objective and authorised scope.
2. Verify connectivity.
3. Discover ports and services.
4. Enumerate relevant services.
5. Validate findings manually.
6. Explain impact and recommend remediation.
7. Record lessons learned.

## Current development

I am continuing to build practical documentation covering vulnerability assessment, CVE/CVSS analysis, Linux, web application testing, and identity and access management.

> **Ethical-use notice:** All security testing documented here was performed against deliberately vulnerable systems in an isolated, authorised home lab.
