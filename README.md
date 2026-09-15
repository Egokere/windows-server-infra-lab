# Enterprise Windows & Network Administration Lab

## Project Overview

This project is a self-built virtualized enterprise IT environment designed to simulate a small corporate network and provide hands-on experience with Windows Server administration, Active Directory, networking, DNS, DHCP, Group Policy, and Linux client connectivity.

The environment is built using VMware and OPNsense, with Windows Server 2025 acting as the primary infrastructure server and Windows 11 and Ubuntu systems acting as clients.

The goal of this project is to develop practical systems and network administration skills that translate to enterprise IT, infrastructure, and cloud support roles.

---

## Current Architecture
      
<img width="5804" height="3728" alt="image" src="https://github.com/user-attachments/assets/33d48c4b-6650-4aab-8586-df1e7698ca0e" />


### Blueprint
* **Operating System:** Windows Server 2025 Evaluation (Desktop Experience)
* **Hypervisor Platform:** VMware Workstation Pro
* **Root Domain Boundary:** corp.local
* **Network Space / Subnet:** 192.168.10.0 /24
* **Primary Domain Controller:** DC01 (192.168.10.10)

---

## Implemented Services & Configurations

### 1. Active Directory Domain Services (AD DS)
* Promoted **DC01** to a root Forest Domain Controller to establish a centralized authentication boundary.
* Built a standardized corporate hierarchy utilizing Organizational Units (OUs) to segregate management boundaries:
## corp.local
 * CorpCompany (Master OU):
  * IT (Administrative Profiles)
  * HR (Human Resources Personnel)
  * Sales (Sales Personnel Accounts)
  
**  Filled directory with user identities and standard usernames. Example: Dsalvatore for user: Damon Salvatore.

### 2. Centralized Name Resolution (DNS)
* **Forward Lookup Zones:** Implemented manual static internal host mapping records to ensure immediate application routing resolution
* **Reverse Lookup Zones:** Established a reverse pointer zone matching the 192.168.10.X pointer format to accommodate local system troubleshooting and security auditing.

### 3. Automated IP Distribution (DHCP)
* Deployed an authorized DHCP pool named CorpClientPool covering the 192.168.10.100 to 192.168.10.200 range.
* Configured an initial exclusion pocket (192.168.10.100 to 192.168.10.105) to safeguard future local static infrastructure against network IP conflicts.
* Added structural scope parameters instructing clients to route through default exit point 192.168.10.1 and query 192.168.10.10 for domain lookups.

### 4. Group Policy Object Enforcements (GPO)
* Designed a custom policy named Desktop Restrictions Policy targeting end-user workspaces.
* Enforced localized workplace security policies:
  * **User Configuration -> Administrative Templates -> Control Panel:** Enabled "Prohibit access to Control Panel and PC settings".
  * **User Configuration -> Administrative Templates -> Desktop:** Enabled "Remove Recycle Bin icon from desktop".
* Explicitly linked the GPO onto the Sales OU node to verify inheritance mechanics across isolated departments.


### 5. Client VM machines
* Created additional VM clients including Windows 11 and Ubuntu Linux machines
* Integrated additional VM clients to the active network. configuring network adapters to engage default gateway (OPNsense) and DC01
* Configured Machines to only utilize the DHCP in my corp.local network for DHCP lease 

### Troubleshooting Example

During initial configuration, the **Windows client** was moved from the VMware NAT network to the LAB-internal network.

The client initially retained network configuration from the previous DHCP environment.

I used:

ipconfig /release
ipconfig /renew
ipconfig /all

This released the previous DHCP lease and requested new network configuration from the DC01 DHCP server.

The client subsequently received an address from the 192.168.10.0/24 lab network.
Ubuntu Client

### **An Ubuntu VM** has been successfully installed and connected to the LAB-internal network.

Connectivity and DNS were validated using:

ping 192.168.10.1
ping google.com
host corp.local

**Testing confirmed**:

Successful communication with the OPNsense gateway
Successful DNS resolution
Successful Internet connectivity
Successful communication with the lab's domain DNS infrastructure

### **Project Status**
### **Completed**
 VMware virtual environment
 OPNsense installation
 WAN/LAN network segmentation
 Windows Server 2025 installation
 Active Directory Domain Services
 corp.local domain
 DNS configuration
 DHCP configuration
 Windows 11 client
 Windows client DHCP validation
 Active Directory OUs
 Group Policy configuration
 Ubuntu installation
 Ubuntu network connectivity testing
 DNS validation from Ubuntu
 
### **Planned**
 Join Windows client to corp.local
 Join Ubuntu client to Active Directory
 Create additional AD users and security groups
 Configure a Windows file server
 Implement NTFS/share permissions
 Create additional enterprise GPOs
 Document troubleshooting scenarios
 Expand the environment with additional infrastructure services
 
### **Project Goal**:

The long-term goal is to expand this environment into a realistic enterprise infrastructure lab covering:

Networking → Windows Administration → Active Directory → Group Policy → Linux → File Services → Security → Cloud Integration

This project is being built incrementally to develop practical skills in systems administration, network administration, infrastructure support, and cloud engineering.
