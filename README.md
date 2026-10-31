# 🛠️ TCM Practical Help Desk - Hands-on IT Support & Active Directory Lab

Welcome to my repository detailing practical hands-on labs, domain administration, and troubleshooting workflows completed during the **TCM Security Practical Help Desk** course. 

This repository documents the deployment of an enterprise-style VirtualBox lab, Active Directory (AD) administration, Group Policy Object (GPO) deployment, core networking diagnostics, and real-world ticketing lifecycle simulations.

---

## 📐 Lab Architecture & Setup

The entire lab environment was built from scratch within **Oracle VM VirtualBox** to simulate a small enterprise IT environment.

### Environment Topology
* **Domain Controller (DC):** Windows Server 2022 (`DC01` / Domain: `corp.local`)
* **Client Workstation:** Windows 11 Enterprise (`WS01` joined to `corp.local`)
* **Hypervisor:** Oracle VM VirtualBox (Internal Network / NAT adapter setups)
