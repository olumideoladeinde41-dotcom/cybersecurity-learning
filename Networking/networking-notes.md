# Networking Fundamentals

## What is networking?

Networking is the process of connecting computers and other devices so they can communicate and share information.

## IP addresses

An IP address identifies a device on a network.

Example: `192.168.10.50`

## Ports and services

A port is a logical communication endpoint used by a network service.

| Port | Common service |
|---:|---|
| 21 | FTP |
| 22 | SSH |
| 80 | HTTP |
| 443 | HTTPS |
| 3389 | RDP |

An open port normally means that a service is listening and prepared to accept network traffic. It is not automatically proof of a vulnerability.

## TCP and UDP

### TCP

TCP is connection-oriented. It establishes a connection before transmitting data and provides reliable, ordered delivery.

### UDP

UDP is connectionless. It sends data without first establishing a connection and does not guarantee delivery or ordering.

| Protocol | Connection | Delivery |
|---|---|---|
| TCP | Connection-oriented | Reliable and ordered |
| UDP | Connectionless | Faster, without guaranteed delivery |

## Security relevance

Understanding addressing, ports, protocols, and services helps a penetration tester interpret scan results and decide what should be investigated next.
