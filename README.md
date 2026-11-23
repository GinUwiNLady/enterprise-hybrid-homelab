# Enterprise Network & Infrastructure Home Lab

Engineering documentation, topologies, sanitized device configurations, and operational runbooks for my virtualized enterprise networking, security, and hybrid-cloud home lab. Built to mirror production branch refreshes, high-availability failover, perimeter security, and Day-2 network operations.

---

## Architecture Overview

![Enterprise Branch & Security Lab Topology](./diagrams/branch-refresh-ha-topology.png)
*(Source file: [`diagrams/branch-refresh-ha-topology.vsdx`](./diagrams/branch-refresh-ha-topology.vsdx) — Logical & Physical Patch Map)*

The primary lab simulates a resilient multi-router enterprise branch site connected to a remote HQ/ISP over dual WAN links:
- **Access / Distribution Layer:** Dual Cisco Catalyst 2960 switches (`SW1`, `SW2`) linked via **LACP EtherChannel (`Po1`)**, segmented with 802.1Q VLANs, hardened with Port Security and SSHv2 management SVIs.
- **Core / Edge Routing:** Dual Cisco ISR 4331 routers (`R1`, `R2`) running **HSRP v2** with uplink interface tracking for automatic gateway failover across Primary and Backup WAN circuits (`R3-ISP`).
- **Perimeter Security:** **Cisco ASA 5506-X** (`FW1`) enforcing stateful inspection, dynamic PAT, and outbound ACL filtering; **IKEv1 IPsec Site-to-Site VPN** protecting branch-to-HQ traffic; companion VM lab (**Check Point Gaia / OPNsense + Suricata**) for NGFW IDS/IPS and URL filtering.
- **Operations & Lifecycle Services:** Dedicated **TFTP & Syslog server** for config archival, IOS image staging/upgrades, and centralized logging.

---

## Core Technical & Operational Competencies

| Domain | Demonstrated Skills & Lab Implementations |
|---|---|
| **Routing & Switching (CCNA)** | Layer 1–3 troubleshooting, Ethernet cabling/MDIX, IOS CLI administration, MAC table analysis, VLANs, 802.1Q trunking, RSTP, LACP EtherChannel, inter-VLAN routing, OSPF, and static/floating static routing. |
| **High Availability & Failover** | Dual-router **HSRP v2** (priority, preemption, uplink tracking), dual-WAN failover, LACP link redundancy, and documented continuous-ping convergence testing. |
| **Perimeter Security & VPN** | **Cisco ASA 5506-X** stateful inspection, NAT/PAT, extended ACLs, **Site-to-Site IPsec VPN (IKEv1)**, switch Port Security (`err-disabled` recovery), and NGFW **IDS/IPS & URL filtering** (Check Point Gaia / OPNsense VM track). |
| **Lifecycle Refresh & Patching** | Repeatable **Golden Config staging templates**, TFTP config backups, flash validation, Cisco IOS firmware upgrade/rollback procedures, and **10-point config compliance audits**. |
| **Documentation & Ops Standards** | **Microsoft Visio** logical/physical rack & patch diagrams, IPAM addressing tables, structured cutover/failover validation sheets, and executive-ready **Root Cause Analysis (RCA)** reports. |

---

## Lab Build Roadmap

- [x] **Phase 1 — Core Switching & IOS Foundations:** CLI navigation, console/SSHv2 hardening, SVI management, MAC learning/flooding, interface standardization, and broken-link L1/L2 troubleshooting.
- [x] **Phase 2 — Enterprise Branch Refresh & High Availability:** Multi-router (`R1`/`R2`/`R3-ISP`) HSRP v2 failover, LACP EtherChannel, Golden Config staging, TFTP/Syslog operations, IOS patching workflow, and compliance auditing.
- [x] **Phase 3 — Perimeter Security & VPN:** ASA 5506-X stateful firewalling, PAT, ACL enforcement, Site-to-Site IPsec VPN, and companion NGFW VM (Check Point / OPNsense + Suricata IDS/IPS & URL filtering).
- [ ] **Phase 4 — Switching Scale & Dynamic Routing:** Full VLAN/Trunk/RSTP root-bridge tuning, Router-on-a-Stick / L3 SVI routing, multi-area OSPF, and IPv6 dual-stack labs.
- [ ] **Phase 5 — Linux Administration, Hybrid Cloud & Security Ops:** Linux server administration, Azure virtual networking (VNets, NSGs, VPN Gateway), SIEM/log analysis, and Python/Ansible config automation.

---

## Repository Index

| Directory | Contents |
|---|---|
| [`/diagrams`](./diagrams) | Microsoft Visio (`.vsdx`) & exported `.png` logical topologies and physical port-mapping sheets. |
| [`/configs`](./configs) | Sanitized Cisco IOS & ASA running configs plus reusable [`branch-router-golden.cfg`](./configs/templates/branch-router-golden.cfg). |
| [`/ipam`](./ipam) | Subnet allocations, VLAN tables, HSRP VIPs, and WAN point-to-point `/30` records. |
| [`/runbooks`](./runbooks) | IOS upgrade & rollback SOP, 10-point compliance audit checklist, and branch cutover/failover validation sheets. |
| [`/rca`](./rca) | Simulated outage Root Cause Analysis (RCA) write-ups with timelines and preventive standards. |

---

## Lab Infrastructure Environment

- **Simulation & Emulation:** Cisco Packet Tracer, Cisco Modeling Labs (CML-Free)
- **Virtualization & NGFW Track:** Dedicated workstation running Check Point Gaia evaluation / OPNsense (Suricata IDS/IPS) and Linux VMs
- **Documentation & Version Control:** Microsoft Visio, Markdown, Git / GitHub

---

## Security & Compliance Standards Applied

Every staged router and switch in this repository is audited against a mandatory baseline before deployment:
- SSHv2 only (`transport input ssh`, 2048-bit RSA keys); Telnet disabled globally.
- `enable secret`, `service password-encryption`, and local `secret` credentials enforced.
- Legal MOTD banners, console/VTY idle timeouts (`exec-timeout 5 0`), and `logging synchronous`.
- Unused switch ports administratively shut down; active access ports protected with Port Security.
- Centralized Syslog forwarding (`logging host 10.1.1.10`) and verified pre-change TFTP backups.

---

## Operational Change Log

| Date | Phase / Lab | Summary of Changes |
|---|---|---|
| `2026-11-23` | Phase 2 & 3 | Expanded branch topology with dual 4331 HSRP routers (`R1`/`R2`), simulated ISP (`R3-ISP`), `ASA 5506-X` firewall, LACP `Po1`, IPsec VPN, TFTP/Syslog IOS patching SOP, compliance audit checklist, and failover/RCA documentation. |
| `2026-11-21` | Phase 1 | Initialized repository structure, Visio documentation workflow, and baseline CCNA L1–L2 switching labs. |
