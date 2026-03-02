# Hospital Network Infrastructure

Cisco Packet Tracer | Healthcare Enterprise Network

![Status](https://img.shields.io/badge/Status-Complete-brightgreen) ![VLANs](https://img.shields.io/badge/VLANs-13-blue) ![Devices](https://img.shields.io/badge/Devices-72+-orange) ![Compliance](https://img.shields.io/badge/Compliance-HIPAA-red)

## About This Project

I built this network to simulate what a real hospital network looks like at the enterprise level. Having worked in hospital security at Nuvance Health, I wanted to take what I see on the physical side and translate it into a full network design showing how different departments, devices, and systems actually need to be separated from each other.

The network covers 4 buildings, 4 patient floors, and 7 clinical departments. Everything is segmented using VLANs to keep sensitive systems like ICU and EHR isolated from things like guest Wi-Fi and building management.

## Network Layout

The network runs through an ASA firewall and edge router at the perimeter, then into two redundant core Layer 3 switches in the data center. Each building connects directly to the core switches. Building A and C have their own distribution switches for local switching. Buildings B and D connect straight to the core since they're smaller.

| Zone | What's Here |
|---|---|
| Data Center | 2 core switches, ASA firewall, edge router, 8 servers |
| Building A | 4 patient floors, ICU/OR, emergency room |
| Building B | Administration, HR, billing, cafeteria, guest Wi-Fi |
| Building C | Radiology, pathology lab, pharmacy |
| Building D | HVAC and building management systems |

## VLANs

| VLAN | Name | Purpose | Subnet |
|---|---|---|---|
| 10 | Clinical-Staff | Nurse stations, doctor workstations | 192.168.10.0/24 |
| 20 | Admin | HR, billing, front desk | 192.168.20.0/24 |
| 30 | IoT-Medical | Patient monitors, infusion pumps | 192.168.30.0/24 |
| 40 | VoIP | IP phones throughout the hospital | 192.168.40.0/24 |
| 50 | Guest-WiFi | Patient and visitor internet access | 192.168.50.0/24 |
| 60 | Radiology | PACS imaging systems | 192.168.60.0/24 |
| 70 | Pharmacy | Medication dispensing systems | 192.168.70.0/24 |
| 80 | Lab | Lab equipment and pathology systems | 192.168.80.0/24 |
| 90 | ER | Emergency department | 192.168.90.0/24 |
| 100 | ICU-OR | ICU and OR, most restricted VLAN | 192.168.100.0/24 |
| 110 | Facilities | HVAC, elevators, building systems | 192.168.110.0/24 |
| 120 | Management | Switch and router management only | 192.168.120.0/24 |
| 200 | DMZ | All servers | 10.0.0.0/24 |

## Servers

| Server | Role |
|---|---|
| EHR-Server (10.0.0.10) | Electronic health records |
| AD-Server (10.0.0.11) | Active Directory and user authentication |
| DHCP-Server (10.0.0.12) | IP addressing across all VLANs |
| DNS-Server (10.0.0.13) | Internal name resolution |
| Syslog-Server (10.0.0.14) | Log collection for HIPAA audit trail |
| Intranet-Server (10.0.0.15) | Internal hospital web portal |
| Backup-Server (10.0.0.16) | Nightly backups and disaster recovery |
| PACS-Server (10.0.0.17) | Radiology imaging storage |

## Security

The security design is based on HIPAA requirements and practical experience from working in hospital security. The main idea is that not everything on the network should be able to talk to everything else.

Guest devices are completely blocked from internal systems and can only reach the internet. Medical IoT devices are isolated and only allowed to send logs to the Syslog server. ICU and OR workstations can only reach the EHR server and Active Directory. Facilities and HVAC systems have no access to any clinical VLAN. Every VLAN sends logs to the Syslog server to satisfy the HIPAA audit trail requirement.

On the infrastructure side, the dual core switches eliminate any single point of failure, Active Directory controls access to the EHR, and network segmentation keeps PHI off shared paths.

**Note on simulation limitations:** ACL application to SVI interfaces has a known limitation in Cisco Packet Tracer 9.x. The ACLs are fully written and verified through `show ip access-lists`. In a production environment they would be applied directly to each VLAN interface. The WS-C3560 also does not support telephony-service in CPT 9.x, so VoIP call registration was documented rather than simulated. Phones are correctly placed on VLAN 40 and receive DHCP addresses.

## Device Count

Router x1, Firewall x1, Layer 3 Switches x4, Layer 2 Switches x13, Servers x8, Workstations x28, IP Phones x12, Laptops x2, Printers x2, Access Points x1. Total approximately 72 devices.

## Repo Contents

The diagrams folder has screenshots from each phase of the build showing VLANs, DHCP bindings, ACL configurations, the ASA firewall setup, and the full topology. The configs folder has running configs exported from the core switches and firewall. The packet tracer file is the full working project file. The design doc goes into detail on the architecture decisions, IP addressing scheme, HIPAA compliance mapping, and testing plan.

## Progress

Phase 1 through Phase 10 complete.
