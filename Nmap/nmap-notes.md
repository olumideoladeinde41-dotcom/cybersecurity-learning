# Nmap Basics

## What is Nmap?

Nmap is a network-scanning tool used to discover hosts, identify open ports, and enumerate services.

## Core concepts

- An **open port** indicates that a service is listening for traffic.
- A service can be identified by its port and network response.
- Version detection helps identify the software behind a service.
- An open port alone does not prove that a vulnerability exists.

## Basic scan

```bash
nmap 192.168.1.10
```

This scans common TCP ports on the target.

## Service-version detection

```bash
nmap -sV 192.168.1.10
```

The `-sV` option asks Nmap to identify the service and its likely software version.

## Default scripts and a selected port

```bash
nmap -sC -sV -p 21 192.168.1.10
```

- `-sC`: runs Nmap's default scripts.
- `-sV`: performs service-version detection.
- `-p 21`: limits the scan to TCP port 21.

## Practical lesson

Scan results are leads, not final conclusions. Findings should be validated manually, assessed in context, and supported with evidence before severity is assigned.
