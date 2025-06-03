
# Secure Enterprise Network Architecture with Dual-Homed eBGP, Multi-Area OSPF, and Centralized Wireless Control 

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

🧩 Devices Included, VLAN Naming, and IP Addressing Plan 

    Click the Image below for full details:


[![Excel Table Screenshot](Snapshots/IP%20address.png)]([files/data.xlsx](https://docs.google.com/spreadsheets/d/1QF83DsqjehpkOu5XXQeB_6MHf-8u66oNltrQUHe3wfg/edit?usp=sharing))

📌 Refer to the spreadsheet for a complete list of IP addresses, interface roles, and link purposes across all interconnects.
