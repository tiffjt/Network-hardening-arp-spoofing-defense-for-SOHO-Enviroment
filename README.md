# 🔐 Network Hardening & ARP Spoofing Defense

> 🧠 A hands-on cybersecurity lab demonstrating how to detect, prevent, and mitigate ARP spoofing attacks through Layer 2 network hardening using Cisco configurations and security tools.

![Network Diagram](images/topology-diagram.png)

---

## 📖 Overview

This project showcases practical techniques to strengthen enterprise networks against ARP spoofing, unauthorized access, and other Layer 2 threats.  
Using **Cisco Packet Tracer** and **Wireshark**, the lab demonstrates how VLAN segmentation, DHCP Snooping, and Dynamic ARP Inspection (DAI) work together to prevent malicious ARP manipulations.

---

## 🎯 Objectives

- Understand the mechanics of ARP spoofing and its risks  
- Implement preventive configurations using Cisco IOS  
- Capture and analyze network traffic pre- and post-hardening  
- Validate mitigation success through Wireshark and CLI results  

---

## 🧰 Tools & Technologies

| Category | Tools & Technologies |
|-----------|----------------------|
| **Simulation & Config** | Cisco Packet Tracer / GNS3 |
| **Monitoring & Analysis** | Wireshark |
| **Firewalls & IDS/IPS** | pfSense, Wazuh |
| **Protocols** | VLAN, DHCP Snooping, DAI, Static ARP |
| **OS** | Windows / Linux |
| **Documentation** | Markdown, Draw.io, PDF Reports |

---

## ⚙️ Lab Setup

**Topology Summary:**
- Switch1 (Core Switch)  
- Router (Gateway)  
- PC1 – Admin Network (VLAN 10)  
- PC2 – Guest Network (VLAN 20)

**Network Diagram:**  
![Network Topology](images/topology-diagram.png)

---

## 🧪 Configuration Procedure

1️⃣ **Create VLANs**
```bash
Switch(config)# vlan 10
Switch(config-vlan)# name Admin_Net
Switch(config)# vlan 20
Switch(config-vlan)# name Guest_Net
2️⃣ Assign VLANs to Ports

bash
Copy code
Switch(config)# interface range fa0/1 - 5
Switch(config-if-range)# switchport access vlan 10
Switch(config)# interface range fa0/6 - 10
Switch(config-if-range)# switchport access vlan 20
3️⃣ Enable DHCP Snooping

bash
Copy code
Switch(config)# ip dhcp snooping
Switch(config)# ip dhcp snooping vlan 10,20
Switch(config-if)# ip dhcp snooping trust
4️⃣ Enable Dynamic ARP Inspection

bash
Copy code
Switch(config)# ip arp inspection vlan 10,20
Switch(config-if)# ip arp inspection trust
5️⃣ Apply Port Security

bash
Copy code
Switch(config)# switchport port-security
Switch(config)# switchport port-security maximum 2
Switch(config)# switchport port-security violation restrict
Switch(config)# switchport port-security mac-address sticky
6️⃣ Verification

bash
Copy code
Switch# show ip dhcp snooping
Switch# show ip arp inspection
Switch# show port-security
Switch# show mac address-table
