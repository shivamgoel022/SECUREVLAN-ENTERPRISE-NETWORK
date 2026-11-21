# SECUREVLAN-ENTERPRISE-NETWORK
A secure, segmented enterprise network topology built in Cisco Packet Tracer using VLANs, Router-on-a-Stick, DHCP, NAT/PAT, and Extended ACLs to enforce least-privilege access across departments.
## 📖 Project Overview
This project demonstrates the design and implementation of a secure, segmented enterprise network for a large company. The network isolates untrusted zones (Customers and Guests) from critical internal resources while enforcing least-privilege access between business units.

Built and simulated using **Cisco Packet Tracer**.

## 🎯 Problem Statement
A large company with customers, guests, sales staff, and management operating on a single flat network is vulnerable to lateral movement attacks. A compromised guest device could potentially access sensitive management servers. This project solves that by implementing VLAN segmentation and Access Control Lists (ACLs).

## 🛠️ Technologies & Protocols Used
*   **VLANs (IEEE 802.1Q):** Logical segmentation of the physical network.
*   **Router-on-a-Stick:** Inter-VLAN routing using router sub-interfaces.
*   **DHCP:** Dynamic IP assignment per VLAN.
*   **NAT/PAT:** Network Address Translation (overload) for internet access.
*   **Extended ACLs:** Traffic filtering and security policy enforcement.
*   **Native VLAN Hardening:** Changed to VLAN 999 to prevent VLAN-hopping attacks.
## 🔐 Security Policy Matrix
| Source → / Dest ↓ | Mgmt | Sales | Customer | Guest | Server | Internet |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Management** | ✅ Permit | ✅ Permit | ✅ Permit | ✅ Permit | ✅ Permit | ✅ Permit |
| **Sales** | ❌ Deny | ✅ Permit | ❌ Deny | ❌ Deny | ✅ Permit | ✅ Permit |
| **Customer** | ❌ Deny | ❌ Deny | ✅ Permit | ❌ Deny | ❌ Deny | ✅ Permit |
| **Guest** | ❌ Deny | ❌ Deny | ❌ Deny | ✅ Permit | ❌ Deny | ✅ Permit |

## 🧪 Verification & Testing
All configurations were verified through a comprehensive test suite. 
*   **Guest Isolation:** Guest PC cannot ping Management (ACL 110 blocks it) but can ping `8.8.8.8` (Internet).
*   **Sales Access:** Sales PC can ping the internal Server but cannot ping Management.
*   **ACL Hit Counters:** Verified using `show access-lists` on R1 (proves active dropping of malicious traffic).

## 💡 Key Learning: Stateless ACLs & Echo-Reply
Cisco extended ACLs are **stateless**. When Management pings a Guest, the echo-request is allowed outbound, but the returning echo-reply hits the `deny` rule and is dropped. To fix this, an `ICMP echo-reply permit` was added at the top of each ACL. This allows return traffic for Management-initiated pings without allowing Guests to initiate connections. (This is why enterprise networks use stateful firewalls).

## 🚀 Files Included
*   `SecureVLAN_Topology.pkt` - The Packet Tracer file.
*   `configs/` - Full CLI configurations for R1, ISP, and Switches.
*   `screenshots/` - Verification screenshots.

---
**Author:**  SHIVAM GOEL  
**Course:**  BTECH 
**College:** ABES ENGINEERING COLLEGE GHAZIABAD
