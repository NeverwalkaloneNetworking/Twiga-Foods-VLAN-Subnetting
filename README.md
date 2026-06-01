# Project 2: Enterprise VLAN Segmentation & VLSM Subnetting

## Business Context & Problem Statement
**Twiga Foods Nairobi** was experiencing severe network performance degradation, broadcast storms, and a lack of departmental security due to a completely flat network layout (`10.10.0.0/24`). All departments—Operations, Finance, HR, IT, and Management—shared the same broadcast domain, allowing unauthorized cross-departmental traffic sniffing and unoptimized data transmission.

This deployment systematically slices the flat network block into isolated, secure logical networks (VLANs) using **Variable Length Subnet Masking (VLSM)** to eliminate IP address wastage, using a Core Layer 3 Switch to govern Inter-VLAN routing.



##  Network Topology Architecture
Below is the structural layout designed and verified within Cisco Packet Tracer:

![Twiga Foods Network Topology](twiga-topology.png)

### Core Hardware Components
* **1x Cisco 3560 Multilayer Switch** (`TWIGA-CORE`): Acts as the high-speed routing engine backbone.
* **5x Department Workstations**: Scaled edge hosts mapping to target operational boundaries.



##  VLSM Subnetting Design Matrix
To match strict departmental scaling parameters while preserving finite IP space, the base network (`10.10.0.0/24`) was engineered from the largest host requirement to the smallest:

| Department | Hosts Needed | Designated VLAN | CIDR Block | Subnet Mask | Network ID | Usable IP Range | Broadcast Address |
| :--- | :---: | :---: | :---: | :--- | :--- | :--- | :--- |
| **Operations** | 60 | VLAN 10 | `/26` | `255.255.255.192` | `10.10.0.0` | `10.10.0.2` – `10.10.0.62` | `10.10.0.63` |
| **Finance** | 30 | VLAN 20 | `/27` | `255.255.255.224` | `10.10.0.64` | `10.10.0.66` – `10.10.0.94` | `10.10.0.95` |
| **HR** | 14 | VLAN 30 | `/28` | `255.255.255.240` | `10.10.0.96` | `10.10.0.98` – `10.10.0.110` | `10.10.0.111` |
| **IT** | 10 | VLAN 40 | `/28` | `255.255.255.240` | `10.10.0.112` | `10.10.0.114` – `10.10.0.126` | `10.10.0.127` |
| **Management** | 6 | VLAN 50 | `/29` | `255.255.255.248` | `10.10.0.128` | `10.10.0.130` – `10.10.0.134` | `10.10.0.135` |

*\*Note: The first usable IP address (`.1`, `.65`, `.97`, `.113`, `.129`) of each respective subnet has been strictly allocated to the Switch Virtual Interface (SVI) to serve as the gateway.*

---

## 🛠️ Cisco IOS Configuration Terminal Scripts

### 1. VLAN Logical Database Initialization
```text
TWIGA-CORE> enable
TWIGA-CORE# configure terminal
TWIGA-CORE(config)# hostname TWIGA-CORE

TWIGA-CORE(config)# vlan 10
TWIGA-CORE(config-vlan)# name Operations
TWIGA-CORE(config-vlan)# vlan 20
TWIGA-CORE(config-vlan)# name Finance
TWIGA-CORE(config-vlan)# vlan 30
TWIGA-CORE(config-vlan)# name HR
TWIGA-CORE(config-vlan)# vlan 40
TWIGA-CORE(config-vlan)# name IT
TWIGA-CORE(config-vlan)# vlan 50
TWIGA-CORE(config-vlan)# name Management
TWIGA-CORE(config-vlan)# exit
```
### 2. Physical Edge Interface Access Assignments
```text 
TWIGA-CORE(config)# interface FastEthernet0/1
TWIGA-CORE(config-if)# switchport mode access
TWIGA-CORE(config-if)# switchport access vlan 10

TWIGA-CORE(config)# interface FastEthernet0/2
TWIGA-CORE(config-if)# switchport mode access
TWIGA-CORE(config-if)# switchport access vlan 20

TWIGA-CORE(config)# interface FastEthernet0/3
TWIGA-CORE(config-if)# switchport mode access
TWIGA-CORE(config-if)# switchport access vlan 30

TWIGA-CORE(config)# interface FastEthernet0/4
TWIGA-CORE(config-if)# switchport mode access
TWIGA-CORE(config-if)# switchport access vlan 40

TWIGA-CORE(config)# interface FastEthernet0/5
TWIGA-CORE(config-if)# switchport mode access
TWIGA-CORE(config-if)# switchport access vlan 50
TWIGA-CORE(config-if)# exit
```
### 3. Switch Virtual Interface (SVI) Gateways & Routing Activation
```text 
TWIGA-CORE(config)# interface vlan 10
TWIGA-CORE(config-if)# ip address 10.10.0.1 255.255.255.192

TWIGA-CORE(config)# interface vlan 20
TWIGA-CORE(config-if)# ip address 10.10.0.65 255.255.255.224

TWIGA-CORE(config)# interface vlan 30
TWIGA-CORE(config-if)# ip address 10.10.0.97 255.255.255.240

TWIGA-CORE(config)# interface vlan 40
TWIGA-CORE(config-if)# ip address 10.10.0.113 255.255.255.240

TWIGA-CORE(config)# interface vlan 50
TWIGA-CORE(config-if)# ip address 10.10.0.129 255.255.255.248
TWIGA-CORE(config-if)# exit
! --- Crucial Layer 3 Routing Engine Activation ---
TWIGA-CORE(config)# ip routing
TWIGA-CORE(config)# exit
TWIGA-CORE# write
```
### Functional Testing & Verification Evidence
To validate structural integrity, comprehensive verification commands and ICMP diagnostics were executed.

### Line-Rate Routing Verification
Running show ip route explicitly demonstrates that the switch hardware populates all 5 classless subnets as directly connected (C) virtual networks, creating seamless cross-department path selection.

