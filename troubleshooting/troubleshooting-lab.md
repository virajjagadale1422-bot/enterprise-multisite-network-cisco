# Network Troubleshooting Scenarios

This document contains practical troubleshooting scenarios performed on the enterprise multi-site network using Cisco Packet Tracer.

## Troubleshooting Methodology

The troubleshooting process followed a structured approach: identify the problem, check physical and interface status, verify IP addressing, verify VLAN configuration, verify trunk connectivity, check routing tables, check OSPF neighbors, verify DHCP, verify NAT/PAT, verify ACL rules, test connectivity using ping and traceroute, apply the required fix, and retest connectivity.

---

## Scenario 1 – Incorrect VLAN Assignment

**Problem:** A PC in the HR department could not communicate with its default gateway.
**Symptoms:** The PC either received an incorrect IP address or was unable to communicate with other devices, and the ping to the HR gateway `192.168.10.1` failed. 
**Cause:** The switch port connected to the HR PC was assigned to the wrong VLAN.
**Resolution:** The switch port was configured as an access port and assigned to VLAN 10 using `switchport mode access` and `switchport access vlan 10`.
**Verification:** The configuration was verified using `show vlan brief` and `show interfaces status`, followed by a ping to `192.168.10.1`.
**Result:** The PC was correctly placed in VLAN 10 and successfully communicated with its default gateway.

---

## Scenario 2 – Trunk Configuration Failure

**Problem:** Inter-VLAN communication was not working between users connected to different VLANs.
**Symptoms:** Devices within the same VLAN could communicate, but communication between different VLANs failed even though the router subinterfaces were configured.
**Cause:** The switch interface connecting to the router was not operating as a trunk, preventing VLAN-tagged traffic from reaching the router subinterfaces.
**Resolution:** The router-facing switch port was configured with `switchport mode trunk`.
**Verification:** The trunk was verified using `show interfaces trunk` and `show vlan brief`, and connectivity between different VLANs was tested using ping. 
**Result:** The trunk became operational and VLAN traffic successfully reached the router, restoring inter-VLAN communication.

---

## Scenario 3 – Incorrect Default Gateway

**Problem:** A PC could communicate with devices in its local network but could not reach devices in another VLAN. **Symptoms:** The PC could communicate with local devices, but ping requests to remote VLAN networks failed. **Cause:** The PC had an incorrect default gateway configured. 
**Resolution:** The default gateway was corrected to the appropriate router subinterface, for example `192.168.10.1` for VLAN 10.
**Verification:** The PC IP configuration was checked and connectivity was tested using ping to the local gateway and a remote VLAN device.
**Result:** After correcting the default gateway, the PC successfully communicated with devices in other VLANs.

---

## Scenario 4 – OSPF Neighbor Failure

**Problem:** Router1 could not learn the remote networks connected to Router2.
**Symptoms:** Router1 did not have OSPF routes for the `192.168.40.0/24`, `192.168.50.0/24`, and `192.168.60.0/24` networks.
**Cause:** The OSPF configuration or router-to-router connectivity was incorrect, preventing the OSPF adjacency from forming.
**Resolution:** The Router1 and Router2 WAN interfaces, OSPF network statements, OSPF area, and router IDs were verified and corrected where required. Router1 used `10.0.0.1/30` and Router2 used `10.0.0.2/30`, with both routers configured in OSPF Area 0.
**Verification:** OSPF neighbor status was checked using `show ip ospf neighbor`, and routing information was verified using `show ip route`.
**Result:** The OSPF adjacency reached the `FULL` state and Router1 successfully learned the remote networks through OSPF.

---

## Scenario 5 – DHCP Failure

**Problem:** A PC did not receive an IP address automatically from the router. 
**Symptoms:** The PC did not receive a valid IP address, subnet mask, or default gateway through DHCP.
**Cause:** The DHCP configuration, VLAN assignment, router subinterface, or DHCP pool parameters were incorrect.
**Resolution:** The appropriate DHCP pool was verified and corrected, including the network address, subnet mask, default gateway, and DNS server. For example, VLAN 10 used the `VLAN10-HR` DHCP pool with network `192.168.10.0/24` and default gateway `192.168.10.1`.
**Verification:** DHCP configuration and assigned addresses were checked using `show ip dhcp pool` and `show ip dhcp binding`, followed by renewing the PC's DHCP address and testing connectivity.
**Result:** The PC successfully obtained an IP address from the correct DHCP pool and was able to communicate with the network.

---

## Scenario 6 – NAT/PAT Failure

**Problem:** Internal users could not reach the external network through Router2. 
**Symptoms:** Internal PCs could communicate with their local gateway and internal networks, but traffic to the external network failed.
**Cause:** The required internal network was not correctly matched by the NAT access control list, or the NAT inside/outside interface configuration was incorrect. 
**Resolution:** The NAT configuration was verified and the required internal networks were included in the NAT ACL. Router2 was configured to perform PAT using the outside interface `Serial0/1/0`. 
**Verification:** NAT translations and statistics were checked using `show ip nat translations` and `show ip nat statistics`, while the NAT ACL was verified using `show access-lists`.
**Result:** Internal traffic was successfully translated to the Router2 outside address and external connectivity was restored.

---

## Scenario 7 – Default Route Failure

**Problem:** Internal networks could communicate with each other but could not reach the external network. **Symptoms:** Inter-VLAN and inter-site connectivity worked, but traffic destined for the external network failed. **Cause:** Router2 did not have a valid default route pointing toward the ISP router. **Resolution:** A default static route was configured on Router2 using `ip route 0.0.0.0 0.0.0.0 203.0.113.2`. **Verification:** The routing table was checked using `show ip route`, and connectivity to the ISP router `203.0.113.2` and external network was tested using ping. **Result:** Router2 successfully forwarded unknown/external traffic toward the ISP router and external connectivity was restored.

---

## Scenario 8 – ACL Blocking Traffic

**Problem:** Required traffic between network segments was unexpectedly blocked. 
**Symptoms:** Ping or application traffic between permitted networks failed even though IP addressing and routing were correct.
**Cause:** An ACL rule was incorrectly blocking the required traffic or the required permit statement was missing. **Resolution:** The ACL configuration was reviewed and the permit/deny statements were corrected according to the required security policy.
**Verification:** ACL entries and match counters were checked using `show access-lists`, followed by ping and traceroute tests between the affected networks.
**Result:** Required traffic was successfully permitted while unauthorized traffic remained restricted.

---

## Scenario 9 – Interface Down

**Problem:** A network connection between devices was unavailable.
**Symptoms:** The connected device could not reach the remote network, and the interface status showed that the interface was down or administratively disabled.
**Cause:** The interface was administratively shut down or was not correctly enabled.
**Resolution:** The affected interface was entered into configuration mode and enabled using the `no shutdown` command. **Verification:** Interface status was checked using `show ip interface brief`, and connectivity was tested using ping. The expected interface state was `up/up`.
**Result:** The interface became operational and network connectivity was successfully restored.

---

# Verification Commands Used

The following Cisco IOS commands were used during troubleshooting:

```text
show running-config
show ip interface brief
show vlan brief
show interfaces trunk
show interfaces status
show ip route
show ip ospf neighbor


Final Connectivity Tests

The following connectivity paths were tested:

HR PC → HR Gateway
IT PC → IT Gateway
Server → Server Gateway

Sales PC → Sales Gateway
Finance PC → Finance Gateway
Management PC → Management Gateway

Router1 → Router2
Router2 → ISP Router
Internal PC → External PC
show ip protocols
show ip dhcp binding
show ip dhcp pool
show ip nat translations
show ip nat statistics
show access-lists
ping
traceroute
