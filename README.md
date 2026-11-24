# Enterprise Network & Security Infrastructure Home Lab

Engineering documentation, topologies, sanitized device configurations, and operational runbooks for my virtualized enterprise networking, perimeter security, and hybrid-cloud home lab. Built to mirror production branch refreshes, full-stack high-availability failover, zero-trust zone segmentation, and Day-2 network security operations.

---

## Architecture Overview

![Enterprise Branch & Security Lab Topology](./diagrams/branch-refresh-ha-topology.png)
*(Source file: [`diagrams/branch-refresh-ha-topology.vsdx`](./diagrams/branch-refresh-ha-topology.vsdx) — Logical & Physical Patch Map)*

The lab simulates a resilient multi-router enterprise branch site connected to a remote HQ/ISP over dual WAN circuits, paired with a dedicated NGFW security VM environment:
- **Access & Distribution Layer:** Dual Cisco Catalyst 2960 switches (`SW1`, `SW2`) linked via **LACP EtherChannel (`Po1`)**, segmented with 802.1Q VLANs, and hardened with Port Security and SSHv2 management SVIs.
- **Core & Edge Routing:** Dual Cisco ISR 4331 routers (`R1`, `R2`) running **HSRP v2** with uplink interface tracking (`decrement 20`) for automatic gateway failover across Primary and Backup WAN circuits (`R3-ISP`).
- **3-Zone Perimeter & Firewall High Availability:** Redundant **Active/Standby firewall pair (`FW1`/`FW2`)** implementing **Inside**, **Outside**, and **DMZ** segmentation (Cisco ASA Failover / OPNsense CARP + `pfsync`). Hosts an isolated Linux web server/jump host in the DMZ with strict East-West and North-South access controls—eliminating single points of failure at the security edge.
- **Modern VPN & Layer 7 NGFW Track:** **IKEv2 Site-to-Site IPsec VPN** secured with modern cryptography (`AES-256-GCM`, `SHA-256/384`, `DH Group 19/20`), complemented by a workstation VM track (**OPNsense + Suricata IPS**, **Check Point Gaia**, and evaluation **FortiGate / Palo Alto PAN-OS** VMs) for Layer 7 Application Control, IPS threat prevention, Geo-IP blocking, and URL filtering.
- **Operations & Lifecycle Services:** Dedicated **TFTP & Syslog server** for config archival, IOS image staging/upgrades, and centralized event logging.

---

## Core Technical & Operational Competencies

| Domain | Demonstrated Skills & Lab Implementations |
|---|---|
| **Routing & Switching (CCNA)** | Layer 1–3 troubleshooting, Ethernet cabling/MDIX, IOS CLI administration, MAC table analysis, VLANs, 802.1Q trunking, RSTP, LACP EtherChannel, inter-VLAN routing, OSPF, and static/floating static routing. |
| **Full-Stack High Availability** | Dual-router **HSRP v2** (priority, preemption, uplink tracking), LACP link redundancy, **Active/Standby Firewall HA** (ASA Failover / OPNsense CARP + `pfsync`), and stateful failover validation proving active SSH/HTTPS sessions survive edge failures. |
| **Zone Segmentation & Modern VPN** | **3-Zone Architecture (Inside / Outside / DMZ)** with strict East-West inspection (`DMZ → Inside` explicitly denied), stateful PAT/NAT, extended ACLs, and **IKEv2 Site-to-Site IPsec VPN** (`AES-256-GCM`, `SHA-256/384`, `DH Group 19/20`, verified via `show crypto ikev2 sa` and `show crypto ipsec sa`). |
| **Layer 7 NGFW & Threat Prevention** | Application-layer filtering (App-ID / blocking SSH over non-standard ports), **Suricata IPS** signature detection/blocking (Nmap & exploit testing), Geo-IP blocking, and URL filtering across **OPNsense**, **Check Point Gaia**, and **FortiGate / PAN-OS** evaluation tracks. |
| **Lifecycle Refresh & Patching** | Repeatable **Golden Config staging templates**, TFTP config backups, flash validation, Cisco IOS firmware upgrade/rollback SOPs, and **10-point config compliance audits**. |
| **Documentation & Ops Standards** | **Microsoft Visio** logical/physical rack & patch diagrams, IPAM addressing plans, structured cutover/failover validation sheets, and executive-ready **Root Cause Analysis (RCA)** reports. |

---

## Lab Build Roadmap

- [x] **Phase 1 — Core Switching & IOS Foundations:** CLI navigation, console/SSHv2 hardening, SVI management, MAC learning/flooding, interface standardization, and broken-link L1/L2 troubleshooting.
- [x] **Phase 2 — Enterprise Branch Refresh & Routing HA:** Multi-router (`R1`/`R2`/`R3-ISP`) HSRP v2 failover, LACP EtherChannel, Golden Config staging, TFTP/Syslog operations, IOS patching SOP, and 10-point compliance auditing.
- [x] **Phase 3A — Perimeter Firewall Baseline:** Cisco ASA 5506-X stateful inspection, dynamic PAT, outbound ACL enforcement, and baseline site-to-site IPsec VPN labbing.
- [ ] **Phase 3B — Enterprise Security Hardening (In Progress):**
  - [ ] Upgrade IPsec Site-to-Site VPN from legacy IKEv1 to **IKEv2** (`AES-256-GCM`, `SHA-256/384`, `DH Group 19/20`) with Phase 1/Phase 2 CLI verification logs.
  - [ ] Deploy **3-Zone DMZ architecture** (`Inside` / `Outside` / `DMZ`) hosting an isolated Linux web server/jump host with verified East-West/North-South isolation rules.
  - [ ] Implement **Active/Standby Firewall HA** (`FW1`/`FW2` via ASA Failover or OPNsense CARP + `pfsync`) and execute live stateful SSH/HTTPS failover testing.
  - [ ] Document **Layer 7 NGFW policy tests** (blocking non-standard-port SSH via App-ID, Suricata IPS Nmap/exploit blocks, and Geo-IP filtering) with log screenshots in `/evidence`.
- [ ] **Phase 4 — Switching Scale & Dynamic Routing:** Full VLAN/Trunk/RSTP root-bridge tuning, Router-on-a-Stick / L3 SVI routing, multi-area OSPF, and IPv6 dual-stack labs.
- [ ] **Phase 5 — Linux Administration, Hybrid Cloud & Security Ops:** Linux server hardening, Azure virtual networking (VNets, NSGs, VPN Gateway), SIEM/log analysis, and Python/Ansible config automation.

---

## Repository Index

| Directory | Contents |
|---|---|
| [`/diagrams`](./diagrams) | Microsoft Visio (`.vsdx`) & exported `.png` logical topologies and physical port-mapping sheets. |
| [`/configs`](./configs) | Sanitized Cisco IOS, ASA, and NGFW running configs plus reusable [`branch-router-golden.cfg`](./configs/templates/branch-router-golden.cfg). |
| [`/ipam`](./ipam) | Subnet allocations (Inside, DMZ, WAN `/30` transits), VLAN tables, HSRP/CARP VIPs, and physical patch maps. |
| [`/runbooks`](./runbooks) | IOS upgrade & rollback SOP, 10-point compliance audit checklist, and router/firewall failover validation sheets. |
| [`/evidence`](./evidence) | Verification outputs and log screenshots for IKEv2 SA states, DMZ zone isolation tests, stateful firewall failover, and L7 NGFW/Suricata IPS blocks. |
| [`/rca`](./rca) | Simulated outage Root Cause Analysis (RCA) write-ups with timelines and preventive standards. |

---

## Lab Infrastructure Environment

- **Network Simulation & Emulation:** Cisco Packet Tracer, Cisco Modeling Labs (CML-Free)
- **Virtualization & NGFW Track:** Dedicated hypervisor workstation running **OPNsense** (CARP HA + Suricata IPS), **Check Point Gaia** evaluation, Linux DMZ targets, and expansion VMs (**FortiGate / Palo Alto PAN-OS** eval)
- **Documentation & Version Control:** Microsoft Visio, Markdown, Git / GitHub

---

## Security & Compliance Standards Applied

Every staged router, switch, and firewall in this repository is audited against a mandatory baseline before deployment:
- **Management Plane:** SSHv2 only (`transport input ssh`, 2048-bit RSA keys); Telnet disabled globally; `enable secret`, `service password-encryption`, and Type 5/8/9 local `secret` accounts enforced.
- **Cryptography:** IKEv2 only for site-to-site IPsec tunnels (`AES-256-GCM`, `SHA-256/384`, `DH Group 19/20`); legacy IKEv1, 3DES, MD5, and SHA-1 prohibited.
- **Zone Segmentation:** Strict 3-zone least privilege (`Outside → DMZ` limited to explicit service ports, `DMZ → Inside` explicitly denied, `Inside → DMZ` restricted to admin protocols).
- **Resilience & Hygiene:** Active/Standby gateway and firewall failover validated under active sessions; unused switch ports shut down; legal MOTD banners, idle timeouts (`exec-timeout 5 0`), centralized Syslog (`10.1.1.10`), and pre-change TFTP backups verified.

---

## Operational Change Log

| Date | Phase / Lab | Summary of Changes |
|---|---|---|
| `2026-11-24` | Phase 3B Plan | Upgraded security roadmap and architecture standards to include IKEv2 modern IPsec crypto, 3-zone DMZ East-West segmentation, Active/Standby firewall HA (`FW1`/`FW2`), and Layer 7 NGFW/IPS test evidence tracking. |
| `2026-11-23` | Phase 2 & 3A | Expanded branch topology with dual 4331 HSRP routers (`R1`/`R2`), simulated ISP (`R3-ISP`), `ASA 5506-X` firewall, LACP `Po1`, TFTP/Syslog IOS patching SOP, compliance audit checklist, and failover/RCA documentation. |
| `2026-11-21` | Phase 1 | Initialized repository structure, Visio documentation workflow, and baseline CCNA L1–L2 switching labs. |
