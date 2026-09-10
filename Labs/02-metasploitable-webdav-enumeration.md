# Hands-On Lab — Metasploitable 2 WebDAV Enumeration

## Objective

Enumerate the HTTP service on Metasploitable 2, identify exposed web applications and WebDAV capabilities, and verify the WebDAV response safely without modifying the target.

## Scope and environment

| Role | System | Address | Network |
|---|---|---|---|
| Attacker | Ubuntu | `192.168.56.101` | VirtualBox host-only |
| Target | Metasploitable 2 | `192.168.56.102` | VirtualBox host-only |

The target was deliberately vulnerable and isolated from the public network.

## 1. HTTP header enumeration

```bash
curl -I http://192.168.56.102/
```

The response identified:

- Apache HTTP Server 2.2.8 on Ubuntu.
- DAV/2 support.
- PHP 5.2.4.

## 2. Web-content discovery

The default page exposed links to several deliberately vulnerable applications and directories:

- `/twiki/`
- `/phpMyAdmin/`
- `/mutillidae/`
- `/dvwa/`
- `/dav/`

## 3. Supported WebDAV methods

```bash
curl -i -X OPTIONS http://192.168.56.102/dav/
```

The server advertised methods including:

```text
OPTIONS, GET, HEAD, POST, DELETE, TRACE, PROPFIND, PROPPATCH,
COPY, MOVE, LOCK, UNLOCK
```

This demonstrated that WebDAV functionality was enabled. Advertising a method does not by itself prove that unauthorised modification is possible.

## 4. Safe PROPFIND validation

A request using infinite depth returned HTTP 403. I then limited the request to the current resource:

```bash
curl -i -X PROPFIND -H "Depth: 0" http://192.168.56.102/dav/
```

The server returned HTTP `207 Multi-Status` with XML metadata for the `/dav/` collection. This confirmed that WebDAV resource enumeration was available at depth zero.

## Finding

| Field | Result |
|---|---|
| Finding | WebDAV enabled and enumerable |
| Target | `192.168.56.102` |
| Path | `/dav/` |
| Server | Apache 2.2.8 DAV/2 |
| Evidence | OPTIONS response and PROPFIND HTTP 207 |
| Status | Confirmed |

### Potential impact

WebDAV expands the available HTTP methods and attack surface. The real risk depends on authentication and whether write-capable methods are permitted to unauthorised users. This exercise confirmed enumeration only; it did not demonstrate unauthorised file modification.

### Recommendation

Disable WebDAV if it is not required. Otherwise, require authentication, apply least-privilege permissions, restrict unnecessary methods, use HTTPS, and maintain supported server software.

## Lessons learned

- HTTP headers can reveal useful technology information.
- OPTIONS identifies advertised methods, not necessarily exploitable permissions.
- PROPFIND can enumerate WebDAV resource metadata.
- HTTP 207 is a normal WebDAV multi-status response.
- Claims should remain limited to what the evidence proves.
- Read-only validation can reduce unnecessary changes to a target.

## Tools used

VirtualBox, Ubuntu, Metasploitable 2, Nmap, and curl.

> All testing was performed in an authorised, isolated environment.
