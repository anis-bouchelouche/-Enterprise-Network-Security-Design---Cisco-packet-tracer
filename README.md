# Enterprise Network Design – Cisco Packet Tracer

## Overview

This repository contains my implementation of a full enterprise network, built and simulated in **Cisco Packet Tracer 9.0.0**. The goal was to design something that could plausibly sit inside a real organization — dual-stack addressing, hardened switching, layered perimeter defenses, and centralized management — all configured and verified end-to-end.

The topology is split into two connected environments:

1. An **IPv4-only internal network**, with strict access control and centralized monitoring.
2. A **dual-stack (IPv4/IPv6) services network**, hosting the organization's public-facing and internal application servers.

---

## Topology

<p align="center">
  <img src="topology.png" alt="Network Topology Diagram" width="900">
</p>

---

## Architecture

### Internal Network (IPv4-only)

| Component | Detail |
|---|---|
| Addressing | `10.11.12.0/24` |
| Host addressing | DHCP |
| Routing | OSPFv2 |
| Management services | NTP (authenticated), Syslog, AAA |

### Services Network (Dual-Stack)

| Component | Detail |
|---|---|
| IPv6 addressing | `2000:12:34::/64` via SLAAC |
| IPv4 addressing | Used for routing and service reachability |
| Routing | OSPFv2 for IPv4; no OSPFv3 — IPv6 reachability via IPv6-over-IPv4 tunneling |
| Hosted services | DNS, Web (HTTP/HTTPS), Mail (SMTP/POP3), FTP |

All services sit behind defined security policies rather than being openly reachable.

---

## Security

### Layer 2 / Switching
- VLAN segmentation (user, management, trunk)
- 802.1X port-based authentication
- Port Security
- DHCP Snooping
- Dynamic ARP Inspection (DAI)
- STP hardening — PortFast + BPDU Guard
- Unused ports parked in a blackhole VLAN

### Layer 3 / Perimeter
- Zone-Based Policy Firewall (ZBF)
- IPv4 and IPv6 ACLs
- IOS IPS with stateful inspection
- Cisco ASA firewall protecting the DMZ
- Wireless secured with WPA2/WPA3

### AAA / Management
- TACACS+ and RADIUS, with local authentication as fallback
- Role-Based Access Control (RBAC) with custom privilege levels for administrative separation

---

## Inter-Site & IPv6 Connectivity
- Site-to-site **IPSec VPN** between R4 and R6 encrypts inter-domain traffic
- IPv6 is tunneled across the IPv4-only backbone, giving end-to-end IPv6 reachability between domains without native IPv6 routing on the backbone

---

## Objectives

**Dual-stack foundation**
- IPv4 (`10.11.12.0/24`) across all routers and inter-router links
- IPv6 (`2000:12:34::/64`) on the services-side routers
- DHCP for IPv4 hosts, SLAAC for IPv6 hosts
- IPv6-over-IPv4 tunneling between domains

**Secure switching**
- VLANs for user, management, and trunk traffic
- Full Layer 2 hardening (802.1X, Port Security, DHCP Snooping, DAI, STP security)
- Blackhole VLAN for unused ports

**Dynamic routing**
- OSPFv2 across the IPv4-only network, and where needed on the services network
- IPv6 reachability via tunneling
- Static routes for networks behind the ASA

**Core services**
- Authenticated NTP and Syslog on the internal network
- R5 configured as NTP master on the services network
- DNS, Web, Mail, and FTP servers deployed and reachable per policy

**Defense in depth**
- ZBF on R1 controlling ICMP, HTTP/HTTPS, DNS, and FTP
- IOS IPS on R5 with stateful inspection
- IPv6 ACLs on R7 restricting unauthorized inter-VLAN traffic
- ASA policies for INSIDE → DMZ and OUTSIDE → DMZ traffic

---

## Tools & Technologies
- Cisco Packet Tracer 9.0.0
- Cisco IOS routers & switches
- Cisco ASA firewall
- IPv4/IPv6 dual-stack networking
- IPSec VPN

---

## Repository Contents
- `*.pkt` — Cisco Packet Tracer topology file
- `topology.png` — network topology diagram
- Device configuration files — exported running-configs for each router and switch

---

## Getting Started
1. Clone this repository
2. Open the `.pkt` file in **Cisco Packet Tracer 9.0.0** or later
3. Open the configuration files directly to inspect or reuse individual device settings

---

## Author
**[Your Name]**
[GitHub](https://github.com/your-username) • [LinkedIn](https://linkedin.com/in/your-profile)

## License
Shared for educational purposes — feel free to fork and adapt for your own coursework or labs.
