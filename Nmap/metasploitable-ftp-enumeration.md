# Hands-On Lab — Metasploitable 2 FTP Enumeration

## Objective

Apply Nmap enumeration techniques to a deliberately vulnerable Metasploitable 2 VM and manually validate an FTP security finding.

The objectives were to:

- Discover open ports.
- Identify services and versions.
- Enumerate the FTP service.
- Validate a scanner finding manually.
- Document impact and remediation.

## Scope and environment

| Role | System | Address | Network |
|---|---|---|---|
| Attacker | Ubuntu | `192.168.56.101` | VirtualBox host-only |
| Target | Metasploitable 2 | `192.168.56.102` | VirtualBox host-only |

The target was deliberately vulnerable and isolated from the public network.

## 1. Connectivity test

```bash
ping -c 4 192.168.56.102
```

Result: four packets transmitted, four received, and 0% packet loss.

## 2. Initial port scan

```bash
nmap 192.168.56.102
```

The scan identified multiple open TCP ports, including FTP (21), SSH (22), Telnet (23), SMTP (25), DNS (53), HTTP (80), NetBIOS (139), SMB (445), MySQL (3306), PostgreSQL (5432), VNC (5900), and HTTP (8180).

## 3. Service-version detection

```bash
nmap -sV 192.168.56.102
```

Relevant result:

```text
21/tcp open  ftp  vsftpd 2.3.4
```

## 4. Focused FTP enumeration

```bash
nmap -sC -sV -p 21 192.168.56.102
```

Nmap reported that anonymous FTP login was allowed with FTP response code 230. It also indicated that the FTP control and data connections were unencrypted.

## 5. Manual validation

```bash
ftp 192.168.56.102
```

Anonymous authentication succeeded. The server identified the remote system as UNIX and entered binary transfer mode.

After authentication, I ran:

```text
ls
```

The server returned a successful directory-listing response, but no files or directories were visible.

## Finding

| Field | Result |
|---|---|
| Finding | Anonymous FTP authentication enabled |
| Target | `192.168.56.102` |
| Port | `21/tcp` |
| Service | FTP |
| Version | vsftpd 2.3.4 |
| Status | Manually confirmed |

### Potential impact

Anonymous users can authenticate without a normal user account. The actual impact depends on the permissions and resources available to that account. No files were visible in the initial directory during this test.

### Recommendation

Disable anonymous FTP unless there is a documented business requirement. If it is required, restrict permissions, expose only intended resources, and prefer encrypted transfer mechanisms.

## Lessons learned

- An open port does not automatically mean a service is vulnerable.
- Version detection provides useful research leads.
- Nmap scripts can identify potentially insecure configurations.
- Scanner findings should be manually validated.
- Severity depends on demonstrated access, permissions, and impact.
- Testing must remain within an authorised scope.

## Tools used

VirtualBox, Ubuntu, Metasploitable 2, Nmap, and an FTP client.
