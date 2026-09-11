# Windows Server & Active Directory Core Infrastructure Lab

## Project Overview
This project demonstrates the deployment and administration of an isolated, enterprise-grade core network infrastructure built natively on a local hypervisor. The goal of this lab was to transition from an unmanaged workgroup environment to a centralized identity, security, and resource management framework using modern Microsoft infrastructure services.

## Progress Tracking
- [x] Step 1: Install Windows Server 2025 Evaluation VM
- [x] Step 2: Domain Controller Promotion (Domain: corp.local)
- [x] Step 3: Configure DNS Forward & Reverse Lookup Zones
- [x] Step 4: DHCP Scope, Exclusions, & Reservations Setup
- [x] Step 5: Active Directory OU Structure & Test User Creation
- [x] Step 6: Group Policy Object (GPO) Building & Linking
- [x] Step 7: Incident Simulation (Break & Fix Troubleshooting)
- [x] Step 8: Network Diagram Finalization
      

### Core Architectural Blueprint
* **Operating System:** Windows Server 2025 Evaluation (Desktop Experience)
* **Hypervisor Platform:** VMware Workstation Pro
* **Root Domain Boundary:** `corp.local`
* **Network Space / Subnet:** `192.168.10.0 /24`
* **Primary Domain Controller:** `DC01` (`192.168.10.10`)

---

## Implemented Services & Configurations

### 1. Active Directory Domain Services (AD DS)
* Promoted `DC01` to a root Forest Domain Controller to establish a centralized authentication boundary.
* Built a standardized corporate hierarchy utilizing Organizational Units (OUs) to segregate management boundaries:
  ```text
  corp.local
  └── CorpCompany (Master OU)
      |__ IT (Administrative Profiles)
      ├── HR (Human Resources Personnel)
      └── Sales (Sales Personnel Accounts)
  ```
* Seeded directory nodes with mock enterprise user identities configured with standard naming formats (e.g., `jdoe@corp.local`).

### 2. Centralized Name Resolution (DNS)
* **Forward Lookup Zones:** Implemented manual static internal host mapping records to ensure immediate application routing resolution (e.g., `app01.corp.local` -> `192.168.10.50`).
* **Reverse Lookup Zones:** Established a reverse pointer zone matching the `192.168.10.X` pointer format to accommodate local system troubleshooting and security auditing.

### 3. Automated IP Distribution (DHCP)
* Deployed an authorized DHCP pool named `CorpClientPool` covering the `192.168.10.100` to `192.168.10.200` range.
* Configured an initial exclusion pocket (`192.168.10.100` to `192.168.10.105`) to safeguard future local static infrastructure against network IP conflicts.
* Injected structural scope parameters instructing clients to route through default exit point `192.168.10.1` and query `192.168.10.10` for domain lookups.

### 4. Group Policy Object Enforcements (GPO)
* Designed a custom policy named `Desktop Restrictions Policy` targeting end-user workspaces.
* Enforced localized workplace security policies:
  * **User Configuration -> Administrative Templates -> Control Panel:** Enabled "Prohibit access to Control Panel and PC settings".
  * **User Configuration -> Administrative Templates -> Desktop:** Enabled "Remove Recycle Bin icon from desktop".
* **Targeting:** Explicitly linked the GPO onto the `Sales` OU node to verify inheritance mechanics across isolated departments.


