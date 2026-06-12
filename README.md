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

### Project 3: Traffic Segmentation & Inter-VLAN Routing (Upcoming)

Addresses the risk of a flat network architecture by segmenting traffic into distinct logical zones (Corporate, Guest Wi-Fi, Server) using 802.1Q VLAN trunking and router subinterfaces, reducing the exposure of HIPAA-relevant traffic to lower-trust network segments.

**Status:** Planned

### Project 4: Infrastructure Resilience (Upcoming)

Addresses single points of failure in the branch's DHCP and access-control configuration, implementing local DHCP address pools and Access Control Lists (ACLs) to ensure continued availability of mission-critical systems even under partial degradation.

**Status:** Planned

## Repository Structure

```
apex-medical-network-infrastructure/
├── README.md (this file, portfolio overview)
├── project-1-network-baseline/
│   ├── README.md
│   └── topology.png
└── project-2-access-security/
    ├── README.md
    ├── topology.png
    ├── security-violation.png
    └── port-recovery.png
```

## Tools Used

- Cisco Packet Tracer: network simulation and configuration
- Cisco IOS (Catalyst 2960 switch, ISR4331 router): device configuration via CLI
- Markdown / GitHub: engineering documentation and portfolio presentation
