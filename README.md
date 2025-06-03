# Secure Enterprise Network Architecture with Dual Multi-Homed Connectivity

**Protocols & Technology Covered Yet**: HSRP, OSPF Multi-Area, eBGP, DHCP, DNS, Web Server, WLC, NTP, EtherChanell and more

**Note**: I would be updating this documentation as I proceed


## 🎯 Objective  
This lab simulates an enterprise-grade network integrating high availability, dynamic routing, centralized services, and multi-site architecture. It includes:  


- Redundant default gateway using HSRP  
- Dedicated servers at the HQ Server Farm
- Multi-area OSPF intra and inter-site routing  
- Inter-AS communication via eBGP  
- Central DHCP, DNS, and HTTP services  
- Wireless infrastructure Management using WLC  
- Remote User connectivity
  

  ## 🖥️ Topology Diagram  

   ![Alt text](snapshots/TOPOLOGY.png) 

The network design uses a three-tier architecture at HQ and a collapsed core model at branch sites to demonstrate architectural diversity.  
- **Access layer** handles Layer 2 management and security.  
- **Distribution layer** manages local inter-VLAN routing and DHCP.  
- **Core layer** handles multi-area OSPF routing.  
- **Edge routers** run eBGP for inter-site communication with two ISPs.  

This design ensures high availability and eliminates single points of failure from the access layer to the ISP edge.

   ## 🧱 Devices Used  
- Cisco 2911 Routers (x6) :
  - ISP 1, ISP 2
  - 2 HQ Edge Routers
  - 2 Branches Edge ROuters
- Cisco 2960 Switches (x14)
  - 2 HQ Core, 2 HQ Server Core
- Cisco 3702i Light Weight Access Point 
- Cisco 3504 Wireless Lan Controller   
- DNS Server  
- Web Server  
- DHCP Server  
- End Devices (PCs, Laptops, Printers.)  


