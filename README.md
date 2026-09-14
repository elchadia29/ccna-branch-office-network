# CCNA Project: Multi-VLAN Branch Office Network with Router-on-a-Stick & Port Security

## 📌 Project Overview
Designed, configured, and verified a secure, segmented branch office network using Cisco Packet Tracer. This project demonstrates practical enterprise Layer 2/3 concepts including VLAN segmentation, 802.1Q trunking, Inter-VLAN routing via Router-on-a-Stick, centralized DHCP server pools, and Layer 2 access-port hardening using Port Security.

---

## 📐 Network Architecture & Design

| Segment / Role | VLAN ID | Subnet | Gateway IP | Port Range |
| :--- | :--- | :--- | :--- | :--- |
| **Management** | VLAN 10 | `192.168.10.0/24` | `192.168.10.1` | `Fa0/1 - 3` |
| **HR Department** | VLAN 20 | `192.168.20.0/24` | `192.168.20.1` | `Fa0/4 - 6` |
| **Guest Network** | VLAN 30 | `192.168.30.0/24` | `192.168.30.1` | `Fa0/7 - 9` |
| **Trunk Link** | N/A | 802.1Q Tagged | N/A | `SW1 Fa0/10` ↔ `R0 Gig0/0/0` |

---

## 🛠️ Key Technical Implementations

### 1. Layer 2 Segmentation & 802.1Q Trunking
* Segmented broadcast domains into three distinct VLANs to improve security and network performance.
* Configured an 802.1Q trunk link on `SW1 Fa0/10` to forward tagged traffic for VLANs 10, 20, and 30 to `Router0`.

### 2. Router-on-a-Stick Inter-VLAN Routing
* Provisioned logical subinterfaces (`Gig0/0/0.10`, `Gig0/0/0.20`, `Gig0/0/0.30`) on `Router0` to act as default gateways for each VLAN.
* Enabled 802.1Q encapsulation per subinterface to handle VLAN tag strip/insert operations.

### 3. Dynamic IP Allocation (Cisco IOS DHCP Server)
* Configured three distinct DHCP pools (`MGMT_POOL`, `HR_POOL`, `GUEST_POOL`) on `Router0`.
* Excluded default gateway addresses (`192.168.x.1`) from dynamic distribution to prevent address conflicts.

### 4. Layer 2 Hardening (Port Security)
* Hardened VLAN 10 management access ports (`Fa0/1 - 3`) using Cisco Port Security.
* Configured **Sticky MAC Address Learning** to automatically record connected hardware MACs into `running-config`.
* Applied **Restrict Violation Mode** to drop unauthorized frames, generate real-time Syslog events (`%PORT_SECURITY-2-PSECURE_VIOLATION`), and keep the port operational without disruption.

---

## ✅ Verification & Testing Results

1. **Cross-VLAN Reachability:** Verified ICMP end-to-end communication across all subnets using `ping` and confirmed multi-hop routing using `tracert`.
2. **Security Violation Testing:** Connected an unauthorized rogue host to `Fa0/1`. Verified that frames were immediately dropped, Syslog alerts were triggered, and authorized traffic resumed cleanly upon reconnecting the legitimate host.

---

## 📄 Device Configurations
Full exported running configurations for all network nodes are available in this repository:
* [`SW1-running-config.txt`](./SW1-running-config.txt)
* [`Router0-running-config.txt`](./Router0-running-config.txt)
* [`Topology File (.pkt)`](./CCNA_Branch_Office_Network.pkt)
