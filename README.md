# Hospital Network Infrastructure
**Cisco Packet Tracer | Healthcare Enterprise Network**

![Status](https://img.shields.io/badge/Status-In%20Progress-yellow)
![VLANs](https://img.shields.io/badge/VLANs-13-blue)
![Devices](https://img.shields.io/badge/Devices-72%2B-green)
![Compliance](https://img.shields.io/badge/Compliance-HIPAA-red)

---

## About This Project

I built this network to simulate what a real hospital network looks like at the enterprise level. Having worked in hospital security at Nuvance Health, I wanted to take what I see on the physical side and translate it into a full network design  showing how different departments, devices, and systems actually need to be separated from each other.

The network covers 4 buildings, 4 patient floors, and 7 clinical departments. Everything is segmented using VLANs to keep sensitive systems like ICU and EHR isolated from things like guest Wi-Fi and building management.



## Network Layout


             INTERNET  ISP 
                   
               Firewall 
                    
               Edge Router 
                    
       DC-Core-SW1 DC-Core-SW2 Redundant 
                         
        Bldg A  Bldg B Bldg C Bldg D


Building What's Here 

Data Center  Core switches, firewall, all servers 
Building A  Main hospital  4 floors, ICU, ER 
Building B Medical offices, admin, cafeteria 
Building C  Radiology, lab, pharmacy 
Building D  Facilities — HVAC, building systems



## VLANs

 VLAN | Name  What It's For  Subnet 

| 10 | Clinical-Staff | Nurse stations, doctor workstations | 192.168.10.0/24 |
| 20 | Admin | HR, billing, front desk | 192.168.20.0/24 |
| 30 | IoT-Medical | Patient monitors, infusion pumps | 192.168.30.0/24 |
| 40 | VoIP | Phones throughout the hospital | 192.168.40.0/24 |
| 50 | Guest-WiFi | Patient and visitor internet only | 192.168.50.0/24 |
| 60 | Radiology | PACS imaging systems | 192.168.60.0/24 |
| 70 | Pharmacy | Medication dispensing systems | 192.168.70.0/24 |
| 80 | Lab | Lab equipment and pathology systems | 192.168.80.0/24 |
| 90 | ER | Emergency department — kept separate | 192.168.90.0/24 |
| 100 | ICU-OR | ICU and OR — most restricted VLAN | 192.168.100.0/24 |
| 110 | Facilities | HVAC, elevators, building systems | 192.168.110.0/24 |
| 120 | Management | Switch/router management only | 192.168.120.0/24 |
| 200 | DMZ | All servers | 10.0.0.0/24 |

---

## Servers

| Server | Role |

| EHR-Server | Electronic health records
| AD-Server | Active Directory / user authentication 
| DHCP-Server | Hands out IPs across all VLANs 
| DNS-Server | Internal name resolution 
| Syslog-Server | Collects logs from everything (HIPAA) 
| Intranet-Server | Internal hospital web portal 
| Backup-Server | Nightly backups, disaster recovery 
| PACS-Server | Radiology imaging storage 

---

## Device Count

| Type  Count 

| Router | 1 |
| Firewall (ASA 5505) | 1 |
| Layer 3 Switches | 4 |
| Layer 2 Switches | 13 |
| Servers | 8 |
| Workstations / PCs | 28 |
| IP Phones | 12 |
| Laptops | 2 |
| Printers | 2 |
| Access Points | 1 |
| **Total** | **~72** |

---

## Security

The main security design decisions came from HIPAA requirements and common sense about what should and shouldn't be able to talk to each other on a hospital network.

Key ACL rules:*
- Guest VLAN can't reach anything internal — internet only
- Medical IoT devices (VLAN 30) are fully isolated, they can only send logs to the Syslog server
- ICU/OR (VLAN 100) can only reach the EHR server and Active Directory  nothing else
- Facilities (HVAC/BMS) has zero access to clinical VLANs
- All VLANs send logs to the Syslog server for the HIPAA audit trail

HIPAA touchpoints:
- Segmentation keeps PHI off shared network paths
- Active Directory controls who can access the EHR
- Syslog captures everything for audit purposes
- Dual core switches cover the availability requirement

---

## Repo Structure

```
hospital-network-infrastructure/
├── README.md
├── /diagrams
│   ├── full-topology.png
│   ├── vlan-map.png
│   └── ip-addressing-table.png
├── /configs
│   ├── Edge-Router.txt
│   ├── ASA-Firewall.txt
│   ├── DC-Core-SW1.txt
│   ├── DC-Core-SW2.txt
│   └── (one file per switch and router)
├── /documentation
│   ├── network-design.md
│   ├── vlan-design.md
│   ├── ip-addressing.md
│   ├── security-policies.md
│   └── hipaa-compliance.md
├── /packet-tracer
│   └── hospital-network.pkt
└── /testing
    └── ping-test-results.md




## Progress

- [x] Phase 1 — Design & Planning
- [ ] Phase 2 — Device Placement & Cabling
- [ ] Phase 3 — VLAN Configuration
- [ ] Phase 4 — IP Addressing
- [ ] Phase 5 — Inter-VLAN Routing
- [ ] Phase 6 — ACL Security Policies
- [ ] Phase 7 — DHCP, DNS & Services
- [ ] Phase 8 — VoIP
- [ ] Phase 9 — Firewall Configuration
- [ ] Phase 10 — Testing & Verification
- [ ] Phase 11 — Documentation

---

## Tools Used

Cisco Packet Tracer, Cisco IOS, Cisco ASA, VLANs / 802.1Q, OSPF, DHCP, DNS, STP, VoIP, ACLs

---

## Author

Thomas Marvin — Security Officer at Nuvance Health, MS Cybersecurity student at University at Albany
