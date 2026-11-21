# Enterprise Physical-to-Cloud Hybrid Home Lab

An end-to-end enterprise infrastructure lab designed, cabled, configured, automated, and documented from Layer 1 structured cabling through hybrid Microsoft Azure integration. This repository serves as living engineering documentation—housing sanitized configurations, Infrastructure as Code (IaC), Microsoft Visio architecture diagrams, and troubleshooting runbooks.

## Architecture & Visual Documentation
*(Layer 1–3 and Hybrid Cloud Microsoft Visio `.vsdx` files and exported `.png` diagrams will be published in `docs/diagrams/` as each phase is built.)*

<!-- Uncomment once your first Visio diagram PNG is uploaded:
![Logical Network Topology](docs/diagrams/l3-routing-security.png)
-->

## Target Infrastructure Stack
- **Layer 1 & Physical Switching:** Structured Cat6 (T568B), 110 keystone patch panel, 1G OM3 multimode fiber SFP uplinks, Cisco Catalyst 3560-CX L2/L3 switches
- **Routing & Emulation:** Cisco Modeling Labs (IOSv/IOS-XE), OPNsense virtual firewall, VLAN segmentation, STP, LACP EtherChannel, OSPF, NAT/PAT, ACLs
- **Virtualization & OS:** Bare-metal Proxmox VE (ZFS), Rocky Linux, Ubuntu Server, Windows Server (Active Directory DS, DNS, DHCP, GPOs)
- **Hybrid Cloud & Automation:** Microsoft Azure (VNets, Microsoft Entra Connect, Site-to-Site IPsec VPN), Terraform, Ansible, Bash/Python
- **Security & Monitoring:** Wazuh SIEM, centralized syslog forwarding (Cisco IOS, firewall, Linux/Windows endpoints), automated active-response blocking
- **Documentation:** Microsoft Visio (`.vsdx`), IPAM/VLAN matrices, cabling schedules, and disaster recovery runbooks

## Build Roadmap
| Phase | Focus Area | Key Deliverables | Status |
| :--- | :--- | :--- | :--- |
| **Phase 1** | Network Core & Layer 1–3 Lab | IPAM/VLAN design, CML routing topologies (OSPF, NAT, ACLs, L2 security), physical cabling & Catalyst switching | In Progress |
| **Phase 2** | Virtualization & Linux Infrastructure | Proxmox VE host, hardened Rocky/Ubuntu VMs, SSH key auth, LVM, `systemd` backup automation | Planned |
| **Phase 3** | Identity & Hybrid Azure Cloud | Active Directory DS, DNS/DHCP, GPO baseline, Entra Connect sync, Azure VNet & Site-to-Site VPN | Planned |
| **Phase 4** | Centralized Security & SIEM | Wazuh SIEM deployment, syslog ingestion, simulated brute-force attack & active-response rules | Planned |
| **Phase 5** | Automation & Enterprise Runbooks | Ansible playbooks, Terraform Azure IaC, complete Visio `.vsdx` set, DR runbook | Planned |

## Lab Hardware Environment
- **Active Workbench:** Custom workstation (Intel i5-14600K, 32 GB DDR5) running virtualized lab topologies, paired with a Surface Laptop 3 acting as a dedicated admin jump box and serial console terminal.
- **Planned Dedicated Expansion:** Always-on bare-metal Proxmox VE mini-PC with dual NICs, two Cisco Catalyst 3560-CX PoE+ L3 switches, a 12-port keystone patch panel, and multimode fiber uplinks.

## Documentation & Security Standards
- **Sanitized Configs:** All published device configurations redact secrets, hashes, and credentials, and replace public IPs with RFC 5737 documentation space (`203.0.113.0/24`).
- **Operational Logging:** Real-world breaks, misconfigurations, and root-cause fixes are tracked in a dedicated change log rather than hiding troubleshooting steps.
