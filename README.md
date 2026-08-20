[root-README.md](https://github.com/user-attachments/files/31245208/root-README.md)
# Apex Medical Supplies: Branch Office Network Infrastructure

## Overview

This repository documents the design, implementation, security hardening, and ongoing evolution of the network infrastructure for Apex Medical Supplies, a fictional 50-user branch office. Each project folder represents a distinct phase of the network's lifecycle, built using Cisco Packet Tracer and documented using a Problem to Constraint to Decision to Outcome framework, the same approach used to justify real infrastructure decisions to stakeholders.

Rather than a series of disconnected exercises, this portfolio tells one continuous story: a branch office that starts with basic connectivity, gets progressively hardened against security threats, and is built out toward a fully segmented, resilient enterprise-style architecture.

## The Business Scenario

Apex Medical Supplies is opening a new branch office that will house approximately 50 employees. As a healthcare-adjacent business, the network must eventually account for HIPAA-relevant data handling, predictable uptime for cloud-based medical records access, and a security posture appropriate for a location with public-facing areas (lobbies, conference rooms) where unmanaged foot traffic is common.

The IT budget is constrained. There is no dedicated on-site network engineer, and licensing for enterprise NAC or SD-WAN platforms is not available. Every design decision in this portfolio reflects working within native Cisco IOS capabilities on commodity hardware (2960 series switch, ISR4331 router).

## Project Roadmap

### [Project 1: Branch Office Network Baseline & Gateway Architecture](./project-1-network-baseline/)

Establishes the foundational LAN and WAN connectivity for the branch. Covers the IP addressing plan, the rationale for separating Layer 2 switching from Layer 3 routing, point-to-point WAN subnetting (/30), and remote management access to the switch. Includes documented troubleshooting of privilege mode errors, STP convergence delay, a subnet boundary misconfiguration, and ARP related initial packet loss, all verified with end-to-end ping testing and TTL analysis.

**Status:** Complete

### [Project 2: Access-Layer Security Hardening](./project-2-access-security/)

Builds directly on Project 1's baseline by addressing a security audit finding: unsecured access-layer ports in public areas. Implements Port Security (sticky MAC binding, violation shutdown), PortFast, and BPDU Guard on end-user ports, while explicitly excluding the router uplink port from this configuration. Includes a live breach simulation (an unauthorized device is connected and automatically locked out within milliseconds) and a documented two-command rollback and recovery procedure.

**Status:** Complete

### [Project 3: VLAN Segmentation & Incident Containment](./project-3-vlan-segmentation/)

Addresses the risk of a flat network architecture by segmenting traffic into distinct logical zones: Corporate (VLAN 10), Guest (VLAN 20), Server (VLAN 30), and Quarantine (VLAN 99). Implements 802.1Q trunking on the router uplink and router-on-a-stick inter-VLAN routing via subinterfaces. Includes a simulated security incident in which a compromised host (PC1) is moved to the Quarantine VLAN and re-addressed, with a before and after comparison. Key finding: VLAN segmentation alone limits the Layer 2 attack surface but does not restrict Layer 3 routed traffic, since the router forwards freely between subinterfaces with no ACLs configured. This gap is the explicit driver for Project 5.

**Status:** Complete

### Project 4: Change Management & Rollback Documentation (Upcoming)

Takes a real configuration change against the Project 3 topology, such as enabling SSH for remote management or adding a VLAN for a new department, and documents it as a formal Change Request: purpose, risk assessment, maintenance window, implementation steps, verification, and an explicit rollback plan.

**Status:** Planned

### Project 5: Infrastructure Resilience (DHCP & ACLs) (Upcoming)

Addresses single points of failure and unrestricted inter-zone access now that the network is segmented. Implements local DHCP address pools per VLAN and Access Control Lists (ACLs) restricting traffic between zones, for example ensuring Guest Wi-Fi cannot reach the Server VLAN and the Quarantine VLAN cannot reach Corporate or Server zones. Directly closes the Layer 3 routing gap identified in Project 3.

**Status:** Planned

### Project 6: Growth / Merger / IP Conflict Scenario (Upcoming)

Apex Medical Supplies is acquiring a smaller branch office, and both networks use the same private IP range (192.168.1.0/24). This project identifies the resulting addressing collision and resolves it through a re-addressing plan and/or NAT, with a documented migration plan that minimizes downtime.

**Status:** Planned

## Repository Structure

```
apex-medical-network-infrastructure/
├── README.md (this file, portfolio overview)
├── project-1-network-baseline/
│   ├── README.md
│   └── topology.png
├── project-2-access-security/
│   ├── README.md
│   ├── topology.png
│   ├── security-violation.png
│   └── port-recovery.png
└── project-3-vlan-segmentation/
    ├── README.md
    └── topology.png
```

## Tools Used

- Cisco Packet Tracer: network simulation and configuration
- Cisco IOS (Catalyst 2960 switch, ISR4331 router): device configuration via CLI
- Markdown / GitHub: engineering documentation and portfolio presentation
