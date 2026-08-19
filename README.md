# Cisco Enterprise Network Design

## Project Overview

This project demonstrates the design, configuration and testing of a multi-site enterprise network for a fictional organisation, **Glance Ltd**, using Cisco Packet Tracer.

The network was designed to support approximately 50 employees across four offices located in London and Manchester while providing wired and wireless connectivity, network segmentation, dynamic routing, automated IP addressing, secure device management and gateway redundancy.

The project focuses on building a scalable and resilient network using technologies commonly found in enterprise environments.

---

## Network Topology

![Enterprise Network Topology](evidence/network-topology.png)

The network consists of four office networks:

- London A
- London B
- Manchester A
- Manchester B

Each office contains a Cisco 2960 switch providing connectivity to wired endpoints and a wireless access point providing connectivity to laptops and other wireless devices.

The offices connect through Cisco 2811 routers using point-to-point links.

Multiple router connections provide alternative paths between locations and improve network resilience.

Manchester A is additionally segmented into separate **Office** and **Factory** networks using VLANs.

---

## Technologies Implemented

- Cisco Packet Tracer
- IPv4 addressing
- VLSM subnetting
- VLANs
- Inter-VLAN routing
- DHCP
- RIPv2 dynamic routing
- HSRP gateway redundancy
- SSH remote administration
- Wired LAN connectivity
- Wireless LAN connectivity
- Point-to-point WAN links
- Network redundancy
- Ping and traceroute testing

---

## IP Addressing Design

The network uses the private address space:

`192.168.0.0/16`

Variable Length Subnet Masking (VLSM) was used to divide the address space into smaller networks.

Office LANs primarily use `/27` networks, providing up to 30 usable host addresses per subnet.

Point-to-point router connections use `/30` networks to minimise unnecessary IP address usage.

| Location | Subnet | Default Gateway |
|---|---|---|
| London A | `192.168.10.0/27` | `192.168.10.1` |
| London B | `192.168.20.0/27` | `192.168.20.1` |
| Manchester A - Office VLAN | `192.168.40.0/27` | `192.168.40.1` |
| Manchester A - Factory VLAN | `192.168.41.0/27` | `192.168.41.1` |
| Manchester B | `192.168.50.0/27` | `192.168.50.1` |

This addressing structure allows additional devices, VLANs and network segments to be introduced without redesigning the entire addressing scheme.

---

## VLAN Segmentation

The Manchester A network was logically segmented using VLANs.

| VLAN | Name | Network |
|---|---|---|
| VLAN 10 | Office | `192.168.40.0/27` |
| VLAN 20 | Factory | `192.168.41.0/27` |

VLAN segmentation separates office and factory traffic into different broadcast domains.

Inter-VLAN routing was configured so authorised traffic can communicate between the two networks.

Testing confirmed successful communication between devices located in VLAN 10 and VLAN 20.

---

## Dynamic Routing — RIPv2

RIPv2 was configured on the routers to dynamically exchange routing information between the different office networks.

RIPv2 was selected because it supports classless routing and works with the VLSM addressing structure used throughout the network.

Routing table and RIP debugging tests confirmed that routers were successfully advertising and learning remote networks.

This allowed devices in London and Manchester to communicate without requiring static routes for every remote network.

---

## DHCP

DHCP pools were configured to automatically provide end devices with network configuration information including:

- IPv4 address
- Subnet mask
- Default gateway
- DNS server

This reduces the administrative overhead associated with manually configuring every PC and laptop.

Testing confirmed that devices successfully received valid IPv4 configuration from the configured DHCP pools.

---

## HSRP Gateway Redundancy

Hot Standby Router Protocol (HSRP) was implemented to provide default gateway redundancy.

Multiple routers share virtual gateway addresses, allowing a standby router to take over if the active router becomes unavailable.

HSRP failover was tested by shutting down the LAN interface of an active router.

Connectivity remained available and the standby router transitioned to the active role.

After the original router interface was restored, the router resumed its active role using HSRP priority and pre-emption.

This demonstrated successful gateway failover and improved network resilience.

---

## SSH Remote Management

SSH was configured on network devices to provide secure remote administrative access.

Testing was performed by initiating an SSH connection from an endpoint in the Manchester network to a router located in the London network.

The connection successfully provided remote CLI access to the router.

> Security note: Authentication credentials used in the Packet Tracer lab are intentionally not documented in this public repository.

---

## Connectivity Testing

Several connectivity tests were performed to verify the network configuration.

### Cross-Site Connectivity

A device in the London A network successfully communicated with a device in Manchester B.

Example:

`192.168.10.0/27 → 192.168.50.0/27`

A second test confirmed communication between Manchester A and London B.

Example:

`192.168.40.0/27 → 192.168.20.0/27`

Successful ICMP responses demonstrated that routing between the different office networks was operating correctly.

---

## Inter-VLAN Testing

Communication was tested between the Manchester A Office and Factory VLANs.

Example:

`VLAN 20 (192.168.41.0/27) → VLAN 10 (192.168.40.0/27)`

Successful ICMP responses confirmed that inter-VLAN routing was functioning correctly.

---

## DHCP Verification

DHCP bindings and endpoint configuration were inspected to verify automatic address allocation.

Clients successfully received:

- Valid IPv4 addresses
- `/27` subnet masks
- Correct default gateways
- DNS configuration

This confirmed successful DHCP operation across the network.

---

## Routing Verification

RIPv2 operation was verified using router routing tables and RIP debugging information.

The routers successfully learned remote networks through RIP and displayed the number of hops required to reach those networks.

Traceroute testing was also used to verify the path taken between remote network devices.

---

## Redundancy Testing

Network redundancy was tested using HSRP.

The test process included:

1. Verify normal end-to-end connectivity.
2. Check the active and standby HSRP router roles.
3. Shut down the active router LAN interface.
4. Repeat the connectivity test.
5. Verify the standby router became active.
6. Restore the original router interface.
7. Verify the preferred router returned to the active role.

Connectivity remained available during the simulated router failure, demonstrating successful gateway redundancy.

---

## Security Considerations

Several security-related measures were incorporated into the network design.

### VLAN Segmentation

Office and factory systems were placed into separate VLANs to reduce unnecessary broadcast traffic and logically separate different parts of the organisation.

### SSH

SSH provides encrypted remote access to network device command-line interfaces instead of relying on insecure remote management protocols.

### Device Authentication

Network devices were configured with password protection for administrative access.

Credentials have not been included in this public repository.

---

## Scalability

The network was designed with future expansion in mind.

The use of the `192.168.0.0/16` private address space combined with VLSM provides sufficient address space for additional:

- Offices
- VLANs
- Routers
- Switches
- End devices

RIPv2 provides a simple dynamic routing solution suitable for the size of this lab network.

For a significantly larger enterprise environment, a more scalable routing protocol such as OSPF could be considered.

---

## Project Files

### Cisco Packet Tracer Lab

The complete network can be opened in Cisco Packet Tracer:

`packet-tracer/Glance ltd.pkt`

### Evidence

Network topology and supporting screenshots are stored in:

`evidence/`

---

## Skills Demonstrated

This project demonstrates practical experience with:

- Enterprise network design
- Cisco router configuration
- Cisco switch configuration
- IPv4 subnetting
- VLSM
- VLAN configuration
- Inter-VLAN routing
- Dynamic routing
- RIPv2
- DHCP configuration
- HSRP
- Gateway redundancy
- SSH administration
- Wireless networking
- Network troubleshooting
- Ping and traceroute
- Routing table analysis
- Network resilience testing
- Cisco Packet Tracer

---

## Network Evaluation

The completed network provides communication between four office locations while maintaining logical segmentation and redundant network paths.

Testing demonstrated successful:

- Cross-site communication
- Dynamic route learning
- DHCP address allocation
- Inter-VLAN communication
- Wireless connectivity
- SSH remote administration
- HSRP gateway failover

The design provides a balance between functionality, scalability, redundancy and complexity appropriate for a medium-sized simulated enterprise environment.

---

## Future Improvements

Possible improvements to the network could include:

- Migrating from RIPv2 to OSPF for greater scalability
- Implementing Access Control Lists (ACLs)
- Adding firewall protection
- Introducing dedicated network services
- Expanding VLAN segmentation
- Implementing additional monitoring and logging
- Strengthening wireless network security
- Adding further switch and link redundancy
