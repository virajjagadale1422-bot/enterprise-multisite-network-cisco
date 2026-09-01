# enterprise-multisite-network-cisco
# Enterprise Multi-Site Network Infrastructure
### Cisco Packet Tracer | Network Engineering Lab

## Overview

Designed and configured an enterprise-style multi-site network
using Cisco Packet Tracer.

The project demonstrates VLAN segmentation, trunking,
inter-VLAN routing, OSPF, DHCP, NAT/PAT, ACLs,
WAN connectivity and network troubleshooting.

## Technologies

- Cisco IOS
- Cisco Packet Tracer
- VLAN
- 802.1Q Trunking
- Inter-VLAN Routing
- OSPF
- DHCP
- NAT/PAT
- ACL
- IPv4
- VLSM
- TCP/IP
- DNS
- Network Troubleshooting

## Network Topology

![Network Topology](topology/network-topology.png)

## VLAN Design

| VLAN | Department | Network |
|------|------------|---------|
| 10 | Users | 192.168.10.0/24 |
| 20 | Users | 192.168.20.0/24 |
| 30 | Servers | 192.168.30.0/24 |
| 40 | Users | 192.168.40.0/24 |
| 50 | Management | 192.168.50.0/24 |
| 60 | Users | 192.168.60.0/24 |

## Routing

Implemented:

- Inter-VLAN routing
- OSPF
- Static/default routing
- WAN connectivity

## Security

Implemented:

- VLAN segmentation
- Extended ACLs
- Management access restrictions
- NAT/PAT

## Services

- DHCP
- DNS
- Internet connectivity

## Verification

Verified network operation using:

- ping
- traceroute
- show vlan brief
- show interfaces trunk
- show ip route
- show ip ospf neighbor
- show ip dhcp binding
- show ip nat translations
- show access-lists

## Troubleshooting

Troubleshooting scenarios included:

1. Incorrect VLAN assignment
2. Trunk configuration failure
3. Incorrect default gateway
4. OSPF neighbor failure
5. DHCP failure
6. NAT failure
7. ACL blocking required traffic

## Project Status

Completed and tested in Cisco Packet Tracer.
