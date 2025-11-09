🔐 Network Hardening and ARP Spoofing Defense Lab

> 🧠 A practical cybersecurity simulation demonstrating how to protect enterprise networks using Access Control Lists (ACLs), device hardening, and ARP spoofing defense in Cisco Packet Tracer.

![Network Diagram](images/network-diagram.png)

---

## 📘 Overview

This lab project simulates a **secure enterprise network** built in **Cisco Packet Tracer**, focusing on:
- Router ACL configuration to protect a File Server  
- Device hardening through secure remote access (SSH only)  
- Port security to mitigate ARP spoofing attacks  
- Network verification through connectivity testing and access control validation  

---

## 🌐 Network Topology

**Network Summary:**
- **LAN 1 (192.168.1.0/24)** – Admin & File Server network  
- **LAN 2 (192.168.2.0/24)** – Wireless clients network  

Each subnet is connected via **Router1**, which enforces access control and segmentation policies.

| Device | Interface | IP Address | Subnet Mask | Gateway | Notes |
|---------|------------|-------------|--------------|----------|--------|
| PC1 | NIC | 192.168.1.10 | 255.255.255.0 | 192.168.1.1 | LAN1 client |
| PC2 | NIC | 192.168.1.11 | 255.255.255.0 | 192.168.1.1 | LAN1 client |
| PC3 | NIC | 192.168.1.12 | 255.255.255.0 | 192.168.1.1 | LAN1 client |
| File Server | NIC | 192.168.1.100 | 255.255.255.0 | 192.168.1.1 | HTTP/FTP/File services |
| Router1 | G0/0/0 | 192.168.1.1 | 255.255.255.0 | — | LAN1 Gateway |
| Router1 | G0/0/1 | 192.168.2.1 | 255.255.255.0 | — | LAN2 Gateway |
| Laptop0 | Wireless | 192.168.2.13 | 255.255.255.0 | 192.168.2.1 | LAN2 client |
| Laptop1 | Wireless | 192.168.2.14 | 255.255.255.0 | 192.168.2.1 | LAN2 client |

---

## 🧱 Router1 – Access Control Configuration

The ACL enforces secure access to the File Server, allowing **LAN1** clients but blocking **LAN2** clients.

```bash
access-list 101 permit tcp 192.168.1.0 0.0.0.255 host 192.168.1.100 eq 21
access-list 101 permit tcp 192.168.1.0 0.0.0.255 host 192.168.1.100 eq 80
access-list 101 deny ip 192.168.2.0 0.0.0.255 host 192.168.1.100
access-list 101 permit ip any any

interface g0/0/1
 ip access-group 101 in
✅ Effect:

LAN1 devices (PC1–PC3) can access File Server via FTP (21) and HTTP (80)

LAN2 devices (Laptop0–Laptop1) are denied access

🔐 Device Hardening Steps
Router1:

Configured SSH-only remote access (Telnet disabled)

Enforced local user authentication with strong credentials

Verified SSH operational with:

bash
Copy code
show ip ssh
Disabled unnecessary services

Switch (Layer 2 Defense):

Implemented Port Security:

bash
Copy code
Switch(config)# interface range fa0/1 - 5
Switch(config-if-range)# switchport port-security
Switch(config-if-range)# switchport port-security mac-address sticky
Switch(config-if-range)# switchport port-security maximum 2
Switch(config-if-range)# switchport port-security violation restrict
Disabled unused ports to reduce attack surface.

🧠 Security Simulation – ARP Spoofing Defense
Simulated an unauthorized device connecting to the network (e.g., rogue PC on port FA0/6).
The switch’s sticky MAC and violation restrict policies prevented spoofing and blocked the port automatically.

🧾 Observation:

The rogue system’s MAC address was not learned, and access was denied.
Legitimate clients retained connectivity without disruption.

🧪 Testing & Verification
Test	Source	Destination	Expected	Actual	Result
Ping	PC1	File Server (192.168.1.100)	Success	Success	✅
FTP	PC2	File Server (192.168.1.100)	Success	Success	✅
HTTP	PC3	File Server (192.168.1.100)	Success	Success	✅
Ping	Laptop0	File Server (192.168.1.100)	Blocked	Blocked	❌
Telnet	Laptop1	File Server (192.168.1.100)	Blocked	Blocked	❌
SSH	PC1	Router1	Success	Success	✅
Telnet	Any	Router1	Blocked	Blocked	❌

Summary:
ACLs and Port Security operated as expected, enforcing access restrictions and mitigating spoofing attempts.

🧩 Project Story
This project was built to simulate layered network defense through:

Router ACL implementation for traffic segmentation

Switch-level ARP spoofing prevention

Endpoint hardening using SSH and service restrictions

Verification through real-time connectivity and attack simulation

It reflects real-world Network Security Analyst tasks such as securing LANs, testing access control, and documenting compliance with CIA triad principles — Confidentiality, Integrity, and Availability.

🧠 Lessons Learned
Strong ACL design isolates and protects critical resources effectively

Port security and sticky MAC learning are simple yet powerful ARP spoofing defenses

SSH hardening is essential for secure device management

Validation and documentation confirm operational success and audit readiness

💡 Future Enhancements
Integrate with Wazuh SIEM for log collection and intrusion detection

Add pfSense firewall at network perimeter

Automate test validation using Python (Netmiko)

📁 Repository Structure
arduino
Copy code
network-hardening-arp-spoofing-defense/
│
├── images/
│   └── network-diagram.png
│
├── configs/
│   └── router1-acl-config.txt
│
├── captures/
│   └── arp-spoofing-test.pcap
│
├── reports/
│   └── Networking_Project_Report_Nasir1.pdf
│
└── README.md
