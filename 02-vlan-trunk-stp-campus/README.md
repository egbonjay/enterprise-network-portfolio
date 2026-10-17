# Project 02: VLAN, Trunking and STP Campus Network

![Status](https://img.shields.io/badge/status-in%20progress-yellow) ![Tool](https://img.shields.io/badge/Cisco-Packet%20Tracer-blue) ![CCNA](https://img.shields.io/badge/CCNA-Domain%202.0-orange)

## 1. Business requirement
Emetonjor Solutions Ltd needs departmental separation (Admin, Sales, Servers, Guest, IoT) to reduce broadcast domains and limit lateral movement.

## 2. Solution summary (3 bullet points)
- 6 VLANs over 802.1Q trunks with an unused native VLAN
- Rapid PVST+ with a deterministic root bridge; PortFast + BPDU Guard on edge ports
- Inter-VLAN routing using a Layer 3 switch (SVIs)

## 3. Topology
![Topology](diagrams/topology.png)

## 4. Addressing & VLAN plan
(table)

## 5. Design decisions (why, not just what)
| Decision | Reason | Alternative considered |
|---|---|---|
| Native VLAN 999 | Mitigates VLAN hopping | Default VLAN 1 (insecure) |

## 6. Implementation
Key configs are in [configs/](configs/). Highlights:
(short code block)

## 7. Verification
| Test | Command | Expected | Result |
|---|---|---|---|
| Trunk up | show interfaces trunk | Gi0/1 trunking, native 999 | Pass |
| Inter-VLAN ping | PC-ADMIN → PC-SALES | Success | Pass |

## 8. Troubleshooting
Links to tickets in [troubleshooting/](troubleshooting/).

## 9. Security considerations
## 10. What I would do in production
## 11. Lessons learned
## 12. CCNA exam objectives covered
- 2.1 Configure and verify VLANs …
