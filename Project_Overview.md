# Enterprise Multi-Site Network Design (Cisco Packet Tracer)

This project is a full enterprise-style network design for five offices:
Boston, Mumbai, New York, London, and Germany. It was built in Cisco Packet Tracer

The design uses routing, switching, security, and redundancy features that are common in real multi-site networks.

---

## Topology Overview

- 5 offices connected through a core router
- Boston and Mumbai as headquarters are connected by 'GRE Tunnel' for direct communication.
- Departments:
  - Finance – VLAN 10
  - HR – VLAN 20
  - Technical – VLAN 30
- DHCP and DNS servers are in vlan 30 of Boston Router
  - DHCP Server - 192.168.0.34/27
  - DNS Server  - 192.168.0.35/27
- Subnetting from `192.168.0.0/16` with atleast 50 usable IPs per site.
- BOSTON(BOR1)
  - vlan10(G0/1.10) - 192.168.0.1/28
  - vlan20(G0/1.20) - 192.168.0.17/28
  - vlan30(G0/1.30) - 192.168.0.33/27
- MUMBAI(MUR1)
  - vlan10(G0/1.10) - 192.168.0.65/28
  - vlan20(G0/1.20) - 192.168.0.81/28
  - vlan30(G0/1.30) - 192.168.0.97/27
- NEWYORK(NYR1)
  - vlan10(G0/1.20) - 192.168.0.129/27
  - vlan20(G0/1.30) - 192.168.0.161/27
- LONDON(LOR1)
  - vlan10(G0/1.20) - 192.168.0.193/27
  - vlan20(G0/1.30) - 192.168.0.225/27
- GERMANY(GER1)
  - vlan10(G0/1.20) - 192.168.1.1/27
  - vlan20(G0/1.30) - 192.168.1.33/27

---

## Key Features

- **VLANs & Inter-VLAN Routing**
  - Separate VLANs for Finance, HR, and Technical
  - Router sub-interfaces with 802.1Q tagging to support inter-vlan routing.
  - **DHCP** in the Technical VLAN of Boston to allocate ip addresses to all the end devices in the enterprise network.
  - **DNS** server in Boston to resolve router hostnames (e.g., `BOR1`)

- **Routing with OSPF (Multi-Area)**
  - **OSPF** used across all routers
  - One area per office (Area 1–5) plus Area 0 as the backbone
  - Manual router IDs and full reachability and stability between all sites

- **Redundancy**
  - **HSRP** gateway redundancy in Boston and Mumbai
  - **Rapid STP** with PortFast and BPDU Guard enabled
  - **PVST+** to load balance traffic across the network
  - **EtherChannel (LACP)** on New York switches to avoid oversubscription
  - Multiple OSPF paths for failover

- **Security**
  - **ACLs** so:
    - Finance(vlan 10) can access other departments
    - Other departments cannot access Finance(vlan 10)
  - **Port security** with violation mode restict on access switches to defend against MAC flooding
  - **SSH** access configured on routers for secure management
  - DHCP and DNS placed behind secured switch ports

- **GRE Tunnel**
  - **GRE** tunnel between Boston and Mumbai headquarters
  - Mask IPs:
    - BOR1 - 192.168.100.1
    - MUR1 - 192.168.100.2
  - Used for direct and stable inter-HQ communication

---

## Files in This Repository

- `enterprise-multi-site-network-design.pkt`  
  Main Cisco Packet Tracer project file.

- `project_overview.txt`  
  Entire details of the project including test plan.

- `report.pdf`  
  Overview of the project with Command outputs and test proofs: VLANs, OSPF neighbors, HSRP, EtherChannel, SSH, ACLs, etc.

- `topology/`
  Screenshot of the toplogy
  

---

## How to Open and Test

1. Open `enterprise-multi-site-network-design.pkt` in **Cisco Packet Tracer**.
## Enable DHCP in all the end devices
2. Check VLANs on switches:
   - `show vlan brief`
   - `show interfaces trunk`
3. Check routing:
   - `show ip ospf neighbor`
   - `show ip route`
4. Check accessability and hostname resolution:
    - `ssh -l barath bor1`
    - `ping BOR1`
5. Test security:
   - Ping from HR/Technical to Finance (should be blocked)
   - Ping from Finance to other departments (should work)
   - `show port-security interface fa0/1`
6. Test redundancy:
   - Shut down active HSRP interface and check failover
   - Verify EtherChannel with `show etherchannel summary`
7. Test tunnel:
   - `show interface tunnel 10`
   - Ping between Boston and Mumbai loopback networks

---

## Concepts Used

- Subnetting and IP planning
- VLANs, trunks, and inter-VLAN routing
- OSPF multi-area design
- HSRP, STP, Rapid STP, EtherChannel
- ACLs, port security, SSH
- GRE tunneling
