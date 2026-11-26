# Enterprise Network & Security Infrastructure Home Lab

**Author:** Moira Ware | *CompTIA Security+, Network+, A+ | Pursuing CCNA & CySA+*  
[LinkedIn](https://linkedin.com) • [Credly](https://credly.com)

Engineering documentation, Visio topologies, sanitized device configurations, and operational runbooks for a virtualized enterprise network, perimeter security, and hybrid-cloud home lab. Built to mirror production branch refreshes, full-stack high-availability (HA) failover, zero-trust zone segmentation, Azure hybrid networking (AZ-104), and Day-2 security operations.

---

## Architecture Overview

![Enterprise Branch & Security Lab Topology](./diagrams/branch-refresh-ha-topology.png)  
*Source file: [`diagrams/branch-refresh-ha-topology.vsdx`](./diagrams/branch-refresh-ha-topology.vsdx) — Logical & Physical Patch Map*

This lab simulates a resilient multi-router enterprise branch connected to a remote HQ/ISP over dual WAN circuits, paired with a dedicated Next-Generation Firewall (NGFW) security VM environment and hybrid Azure cloud connectivity:

* **Access & Distribution Layer:** Dual Cisco Catalyst 2960 switches (`SW1`, `SW2`) bonded via **LACP EtherChannel (`Po1`)**, segmented with 802.1Q VLANs, and hardened with Port Security and SSHv2 management SVIs.
* **Core & Edge Routing:** Dual Cisco ISR 4331 routers (`R1`, `R2`) running **HSRP v2** with uplink interface tracking (`decrement 20`) for sub-second gateway failover across Primary and Backup WAN circuits (`R3-ISP`).
* **3-Zone Perimeter & Firewall HA:** Redundant **Active/Standby firewall pair (`FW1`/`FW2`)** enforcing **Inside**, **Outside**, and **DMZ** segmentation (Cisco ASA Failover / OPNsense CARP + `pfsync`). Hosts an isolated Linux web server/jump host in the DMZ with strict East-West and North-South access policies—eliminating single points of failure at the security edge.
* **Modern VPN & Layer 7 NGFW Track:** **IKEv2 Site-to-Site IPsec VPN** secured with Suite-B cryptography (`AES-256-GCM`, `SHA-256/384`, `DH Group 19/20`), complemented by a hypervisor security track (**OPNsense + Suricata IPS**, **Check Point Gaia**, and evaluation **FortiGate / Palo Alto PAN-OS** VMs) for Layer 7 Application Control, IPS threat prevention, Geo-IP blocking, and URL filtering.
* **Hybrid Cloud & Azure Integration (AZ-104):** Hub-and-Spoke **Azure Virtual Networks (VNets)** linked to the on-premises perimeter via an **Azure Site-to-Site VPN Gateway**, governed by Network Security Groups (NSGs), User-Defined Routes (UDRs), and Microsoft Entra ID RBAC.
* **Telemetry, Packet Analysis & Lifecycle Services:** Centralized **TFTP & Syslog server (`10.1.1.10`)** for configuration archival, IOS image staging/upgrades, and event logging, paired with **Wireshark** packet captures to verify failover convergence and inspect anomalous traffic.

---

## Core Technical & Operational Competencies

| Domain | Demonstrated Skills & Lab Implementations |
| :--- | :--- |
| **Routing & Switching (CCNA)** | Layer 1–3 diagnostics, Ethernet cabling/MDIX, IOS CLI administration, MAC table analysis, VLANs, 802.1Q trunking, RSTP, LACP EtherChannel, inter-VLAN routing, OSPF, and floating static routes. |
| **Full-Stack High Availability** | Dual-router **HSRP v2** (priority, preemption, uplink tracking), LACP link redundancy, **Active/Standby Firewall HA** (ASA Failover / OPNsense CARP + `pfsync`), and stateful failover testing proving live SSH/HTTPS sessions survive edge failures. |
| **Zone Segmentation & VPN** | **3-Zone Architecture (Inside / Outside / DMZ)** with strict East-West isolation (`DMZ → Inside` explicitly denied), stateful NAT/PAT, extended ACLs, and **IKEv2 Site-to-Site IPsec VPN** (`AES-256-GCM`, `SHA-256/384`, `DH Group 19/20`, verified via `show crypto ikev2 sa` and `show crypto ipsec sa`). |
| **Layer 7 NGFW & Threat Ops** | Application-layer filtering (App-ID / blocking SSH over non-standard ports), **Suricata IPS** signature detection/blocking (Nmap & exploit simulations), Geo-IP blocking, URL filtering, and deep packet inspection. |
| **Hybrid Cloud & Azure (AZ-104)** | Hub-and-Spoke **VNet peering**, **NSG** least-privilege rules, **User-Defined Routes (UDRs)** for NVA traffic inspection, **Site-to-Site VPN Gateway** hybrid tunneling, **Microsoft Entra ID** RBAC/Conditional Access, and Azure Monitor/Log Analytics. |
| **Lifecycle & Change Control** | Repeatable **Golden Config staging templates**, TFTP config backups, flash validation, Cisco IOS firmware upgrade/rollback SOPs, Infrastructure as Code (Bicep/Terraform), and **10-point configuration compliance audits**. |
| **Documentation & Governance** | **Microsoft Visio** logical/physical rack & patch diagrams, IPAM addressing plans, structured cutover/failover validation sheets, and executive-ready **Root Cause Analysis (RCA)** reports. |

---

## Security & Compliance Baseline

Every staged router, switch, firewall, and cloud resource in this repository is audited against a mandatory baseline prior to deployment:

* **Management Plane & Identity Hardening:** SSHv2 enforced (`transport input ssh`, 2048-bit RSA keys); Telnet disabled globally; `enable secret`, `service password-encryption`, and Type 5/8/9 local `secret` credentials required; cloud administrative access governed via **Entra ID RBAC** and MFA.
* **Cryptographic Standards:** IKEv2 required for on-prem and Azure Site-to-Site IPsec tunnels (`AES-256-GCM`, `SHA-256/384`, `DH Group 19/20`); legacy IKEv1, 3DES, MD5, and SHA-1 prohibited.
* **Zero-Trust Zone & Cloud Segmentation:** Strict least privilege across on-prem zones (`Outside → DMZ` restricted to explicit service ports; `DMZ → Inside` explicitly denied; `Inside → DMZ` limited to administrative protocols) mirrored in Azure via **NSGs** and **UDRs**.
* **Operational Hygiene:** Active/Standby gateway and firewall failover validated under live sessions; unused switch ports administratively shut down; legal MOTD banners, idle timeouts (`exec-timeout 5 0`), centralized Syslog (`10.1.1.10`), and pre-change TFTP backups verified.

---

## Repository Index

| Directory | Contents |
| :--- | :--- |
| [`/diagrams`](./diagrams) | Microsoft Visio (`.vsdx`) & exported `.png` logical topologies, hybrid-cloud diagrams, and physical port-mapping sheets. |
| [`/configs`](./configs) | Sanitized Cisco IOS, ASA, and NGFW running configs plus reusable [`branch-router-golden.cfg`](./configs/templates/branch-router-golden.cfg) and Azure IaC templates. |
| [`/ipam`](./ipam) | Subnet allocations (Inside, DMZ, WAN `/30` transits, Azure VNet CIDR blocks), VLAN tables, HSRP/CARP VIPs, and physical patch maps. |
| [`/runbooks`](./runbooks) | IOS upgrade & rollback SOP, 10-point compliance audit checklist, and router/firewall failover validation sheets. |
| [`/evidence`](./evidence) | Verification outputs, packet captures, and log screenshots for IKEv2 SA states, Azure VPN Gateway tunnels, DMZ isolation tests, stateful failover, and L7 NGFW/IPS blocks. |
| [`/rca`](./rca) | Simulated outage Root Cause Analysis (RCA) reports with fault timelines, log evidence, and preventive controls. |

---

## Lab Infrastructure Environment

* **Network Simulation & Emulation:** Cisco Packet Tracer, Cisco Modeling Labs (CML-Free)
* **Virtualization & NGFW Track:** Dedicated hypervisor workstation hosting **OPNsense** (CARP HA + Suricata IPS), **Check Point Gaia** evaluation, Linux DMZ targets, and expansion VMs (**FortiGate / Palo Alto PAN-OS** eval)
* **Cloud & Hybrid Track:** Microsoft Azure (VNets, VPN Gateway, NSGs, Entra ID, Azure Monitor)
* **Telemetry & Documentation:** Wireshark, TFTP/Syslog Server, Microsoft Visio, Markdown, Git / GitHub

---

## Lab Build Roadmap

* [x] **Phase 1 — Core Switching & IOS Foundations:** CLI navigation, console/SSHv2 hardening, SVI management, MAC learning/flooding, interface standardization, and L1/L2 broken-link troubleshooting.
* [x] **Phase 2 — Enterprise Branch Refresh & Routing HA:** Multi-router (`R1`/`R2`/`R3-ISP`) HSRP v2 failover, LACP EtherChannel, Golden Config staging, TFTP/Syslog operations, IOS patching SOP, and 10-point compliance auditing.
* [x] **Phase 3A — Perimeter Firewall Baseline:** Cisco ASA 5506-X stateful inspection, dynamic PAT, outbound ACL enforcement, and baseline site-to-site IPsec VPN labbing.
* [ ] **Phase 3B — Enterprise Security Hardening (In Progress):**
  * [ ] Upgrade IPsec Site-to-Site VPN from IKEv1 to **IKEv2** (`AES-256-GCM`, `SHA-256/384`, `DH Group 19/20`) with Phase 1/Phase 2 CLI verification logs.
  * [ ] Deploy **3-Zone DMZ architecture** (`Inside` / `Outside` / `DMZ`) hosting an isolated Linux web server/jump host with verified East-West/North-South isolation rules.
  * [ ] Implement **Active/Standby Firewall HA** (`FW1`/`FW2` via ASA Failover or OPNsense CARP + `pfsync`) and execute live stateful SSH/HTTPS failover testing.
  * [ ] Document **Layer 7 NGFW policy tests** (blocking non-standard-port SSH via App-ID, Suricata IPS Nmap/exploit blocks, and Geo-IP filtering) with log/pcap evidence in `/evidence`.
* [ ] **Phase 4 — Switching Scale & Dynamic Routing:** Full VLAN/Trunk/RSTP root-bridge tuning, Router-on-a-Stick / L3 SVI routing, multi-area OSPF, and IPv6 dual-stack labs.
* [ ] **Phase 5 — Linux Administration, Azure Hybrid Cloud (AZ-104) & Automation:**
  * [ ] Deploy a **Hub-and-Spoke Azure VNet** architecture with non-overlapping CIDR blocks documented in `/ipam`, segmented by **Network Security Groups (NSGs)** and **User-Defined Routes (UDRs)**.
  * [ ] Establish a hybrid **IKEv2 Site-to-Site IPsec tunnel** between the on-premises perimeter (`FW1`/`R1`) and an **Azure VPN Gateway**, verifying end-to-end BGP/static routing and Wireshark packet captures.
  * [ ] Configure **Microsoft Entra ID** custom RBAC roles, MFA enforcement, and Privileged Identity Management (PIM) access reviews.
