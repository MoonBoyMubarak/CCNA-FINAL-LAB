### Configs 

**DHCP config HQ-MAIN**
**Vlans and names**
vlan 10
name Mgmt                                 
vlan 20
name  Phones                               
vlan 30   
name CustCARE 
vlan 40  
name Printers                            
vlan 50  
name Sales                               
vlan 60
name Staff—Wi-fi
vlan 70 
name Guest—Wi-fi
vlan 80
name Acctng                              
vlan 1000 
name UNUSED 
 

! VLAN 10 - Management
ip dhcp pool Mgmt
 network 172.17.1.0 255.255.255.128
 default-router 172.17.1.1
 dns-server 172.18.6.5
 domain-name moonboy.com
 option 43 ip 172.18.1.5

! VLAN 20 - Phones
ip dhcp pool Phones
 network 172.17.2.0 255.255.255.0
 default-router 172.17.2.1
 dns-server 172.18.6.5
 domain-name moonboy.com

! VLAN 30 - CustCARE
ip dhcp pool CustCARE
 network 172.17.3.0 255.255.255.0
 default-router 172.17.3.1
 dns-server 172.18.6.5
 domain-name moonboy.com

! VLAN 40 - Printers
ip dhcp pool Printers
 network 172.17.4.0 255.255.255.0
 default-router 172.17.4.1
 dns-server 172.18.6.5
 domain-name moonboy.com

! VLAN 50 - Sales
ip dhcp pool Sales
 network 172.17.5.0 255.255.255.0
 default-router 172.17.5.1
 dns-server 172.18.6.5
 domain-name moonboy.com

! VLAN 60 - Staff Wi-Fi
ip dhcp pool Staff-WiFi
 network 172.17.6.0 255.255.255.0
 default-router 172.17.6.1
 dns-server 172.18.6.5
 domain-name moonboy.com

! VLAN 70 - Guest Wi-Fi
ip dhcp pool Guest-WiFi
 network 172.17.7.0 255.255.255.0
 default-router 172.17.7.1
 dns-server 172.18.6.5
 domain-name moonboy.com

! VLAN 80 - Accounting
ip dhcp pool Acctng
 network 172.17.8.0 255.255.255.0
 default-router 172.17.8.1
 dns-server 172.18.6.5
 domain-name moonboy.com




### HSRP

Following these STP root bridge configs: HQ-DSW1 spanning-tree vlan 10, 40, 60, 70 priority 24576,spanning-tree vlan 20, 30, 50, 80, priority 28672, and HQ-DSW2 spanning-tree vlan 20,30,50,80 priority 24576, spanning-tree vlan 10,40,60,70 priority 28672 .Now we have to do hsrp config for for redundancy, btw HQ-DSW1 and 2, HQ-DSW1 would be active router for VLANs 10, 40, 60, and 70 and standby for VLANs 20, 30, 50, and 80. HQ-DSW2 would be the active router for VLANs 20, 30, 50, and 80 and standby for VLANs 10, 40, 60, and 70. Note, no preempt for backup routers, and they should all use standby version 2, standby router priorities should be 90, all active router configs should come first before standby in both switches. for example : inter vlan 10, ip address 172.16.1.2 255.255.255.128, standby version 2, standby 10 ip 172.16.1.1, standby 10 priority 110, standby 10 preempt

### 🔧 HQ-DSW1 Configuration (Active for VLANs 10, 40, 60, 70)

interface vlan 10
 ip address 172.17.1.2 255.255.255.128
 standby version 2
 standby 10 ip 172.17.1.1
 standby 10 priority 110
 standby 10 preempt

interface vlan 40
 ip address 172.17.4.2 255.255.255.0
 standby version 2
 standby 40 ip 172.17.4.1
 standby 40 priority 110
 standby 40 preempt

interface vlan 60
 ip address 172.17.6.2 255.255.255.0
 standby version 2
 standby 60 ip 172.17.6.1
 standby 60 priority 110
 standby 60 preempt

interface vlan 70
 ip address 172.17.7.2 255.255.255.0
 standby version 2
 standby 70 ip 172.17.7.1
 standby 70 priority 110
 standby 70 preempt

! Standby (no preempt) for VLANs 20, 30, 50, 80

interface vlan 20
 ip address 172.17.2.2 255.255.255.0
 standby version 2
 standby 20 ip 172.17.2.1
 standby 20 priority 90

interface vlan 30
 ip address 172.17.3.2 255.255.255.0
 standby version 2
 standby 30 ip 172.17.3.1
 standby 30 priority 90

interface vlan 50
 ip address 172.17.5.2 255.255.255.0
 standby version 2
 standby 50 ip 172.17.5.1
 standby 50 priority 90

interface vlan 80
 ip address 172.17.8.2 255.255.255.0
 standby version 2
 standby 80 ip 172.17.8.1
 standby 80 priority 90


### 🛠️ HQ-DSW2 Configuration (Active for VLANs 20, 30, 50, 80)

interface vlan 20
 ip address 172.17.2.3 255.255.255.0
 standby version 2
 standby 20 ip 172.17.2.1
 standby 20 priority 110
 standby 20 preempt

interface vlan 30
 ip address 172.17.3.3 255.255.255.0
 standby version 2
 standby 30 ip 172.17.3.1
 standby 30 priority 110
 standby 30 preempt

interface vlan 50
 ip address 172.17.5.3 255.255.255.0
 standby version 2
 standby 50 ip 172.17.5.1
 standby 50 priority 110
 standby 50 preempt

interface vlan 80
 ip address 172.17.8.3 255.255.255.0
 standby version 2
 standby 80 ip 172.17.8.1
 standby 80 priority 110
 standby 80 preempt

! Standby (no preempt) for VLANs 10, 40, 60, 70

interface vlan 10
 ip address 172.17.1.3 255.255.255.128
 standby version 2
 standby 10 ip 172.17.1.1
 standby 10 priority 90

interface vlan 40
 ip address 172.17.4.3 255.255.255.0
 standby version 2
 standby 40 ip 172.17.4.1
 standby 40 priority 90

interface vlan 60
 ip address 172.17.6.3 255.255.255.0
 standby version 2
 standby 60 ip 172.17.6.1
 standby 60 priority 90

interface vlan 70
 ip address 172.17.7.3 255.255.255.0
 standby version 2
 standby 70 ip 172.17.7.1
 standby 70 priority 90



### OSPF ROUTING



#### HQ-DSW1 OSPF Configuration
router ospf 10
 router-id 1.1.3.1
 network 172.16.0.84 0.0.0.3 area 1
 network 172.16.0.96 0.0.0.3 area 1
 network 172.16.0.7 0.0.0.0 area 1

network 172.17.1.0 0.0.0.127 area 1
network 172.17.2.0 0.0.0.255 area 1
network 172.17.3.0 0.0.0.255 area 1
network 172.17.4.0 0.0.0.255 area 1
network 172.17.5.0 0.0.0.255 area 1
network 172.17.6.0 0.0.0.255 area 1
network 172.17.7.0 0.0.0.255 area 1
network 172.17.8.0 0.0.0.255 area 1

 passive-interface default
 no passive-interface GigabitEthernet1/0/6
 no passive-interface GigabitEthernet1/0/7

#### HQ-DSW2 OSPF Configuration

router ospf 10
 router-id 1.1.3.2
 network 172.16.0.88 0.0.0.3 area 1
 network 172.16.0.92 0.0.0.3 area 1
 network 172.16.0.8 0.0.0.0 area 1

network 172.17.1.0 0.0.0.127 area 1
network 172.17.2.0 0.0.0.255 area 1
network 172.17.3.0 0.0.0.255 area 1
network 172.17.4.0 0.0.0.255 area 1
network 172.17.5.0 0.0.0.255 area 1
network 172.17.6.0 0.0.0.255 area 1
network 172.17.7.0 0.0.0.255 area 1
network 172.17.8.0 0.0.0.255 area 1 

 passive-interface default
 no passive-interface GigabitEthernet1/0/6
 no passive-interface GigabitEthernet1/0/7


#### HQ-CORE1 OSPF Configuration

router ospf 10
 router-id 1.1.2.1
 passive-interface Loopback0

 network 172.16.0.68 0.0.0.3 area 0  
 network 172.16.0.52 0.0.0.3 area 0  
 network 172.16.0.84 0.0.0.3 area 1
 network 172.16.0.88 0.0.0.3 area 1
 network 172.16.0.116 0.0.0.3 area 1
 network 172.16.0.3 0.0.0.0 area 0

 int r g1/1/3,g1/0/1
 ip ospf network point-to-point 





#### HQ-CORE2 OSPF Configuration 
router ospf 10
 router-id 1.1.2.2
passive-interface Loopback0
 network 172.16.0.56 0.0.0.3 area 0
 network 172.16.0.72 0.0.0.3 area 0
 network 172.16.0.92 0.0.0.3 area 1
 network 172.16.0.96 0.0.0.3 area 1
 network 172.16.0.116 0.0.0.3 area 1

 int r g1/1/3,g1/0/1
 ip ospf network point-to-point 

**HQ-R1**

router ospf 10
 router-id 1.1.1.1
 passive-interface Loopback0
 
int r g0/0-1,g0/2/0,g0/3/0
 ip ospf 10 area 0


int r g0/0-1,g0/2/0,g0/3/0
ip ospf network point-to-point

**HQ-R2**
router ospf 10
 router-id 1.1.1.2
 passive-interface Loopback0

int r g0/0-1,g0/2/0,g0/3/0,lo0
ip ospf 10 area 0

int r g0/0-1,g0/2/0,g0/3/0
ip ospf network point-to-point

 let's do ospf for HQ-R1 and HQ-R2 with networks we excluded to remain in area 0. HQ-R1 has 172.16.0.68/30 and .72/30 then HQ-R2 has networks 172.16.0.52/30 and .56/30






### SVR-FARM

Let's run the spanning-tree bridge redundancy. i think I have already configured the vlans here.

**Vlans and their names**

vlan 10
name Mgmt
vlan 20
name Phones
vlan 30
name IT-SUPPORT
vlan 40
name SVR-Wi-fi
vlan 50
name  WEB-SVR
vlan 60
name DNS-SVR
vlan 70
name FTP-SVR
vlan 80
name DHCP-SVR
vlan 1000
name UNUSED


**_One thing I just notced now is how I want to ensure there won't be any addressing overlaps btw the HQ-MAIN and the Server Farm. I would probably continue from where we stopped in HQ-MAIN_**

! VLAN 10 - Mgmt
ip dhcp excluded-address 172.18.1.1 172.18.1.10
ip dhcp pool Mgmt
 network 172.18.1.0 255.255.255.128
 default-router 172.18.1.1
 dns-server 172.18.6.5
 domain-name moonboy.com
 option 43 ip 172.18.1.5

! VLAN 20 - Phones
ip dhcp pool Phones
 network 172.18.2.0 255.255.255.0
 default-router 172.18.2.1
 dns-server 172.18.6.5
 domain-name moonboy.com

! VLAN 30 - IT-SUPPORT
ip dhcp pool IT-SUPPORT
 network 172.18.3.0 255.255.255.0
 default-router 172.18.3.1
 dns-server 172.18.6.5
 domain-name moonboy.com

! VLAN 40 - SVR-WIFI
ip dhcp pool SVR-WIFI
 network 172.18.4.0 255.255.255.0
 default-router 172.18.4.1
 dns-server 172.18.6.5
 domain-name moonboy.com


   

following this stp rule for SVR-DSW1:
spanning-tree vlan 10, 20,30,40 priority 24576
spanning-tree vlan 50,60,70,80 priority 28672  and this rule for SVR-DSW2 : spanning-tree vlan 50,60,70,80 priority 24576
spanning-tree vlan 10,20,30,40 priority 28672. Config HSRP for gateway redundancy, btw SVR-DSW1 and 2, SVR-DSW1 would be active router for VLANs 10, 20, 30 and 40 and standby for VLANs 50,60,70and 80. SVR-DSW2 would be the active router for VLANs 50,60,70 and 80 and standby for VLANs 10,20,30,and 40. Note, no preempt for backup routers, and they should all use standby version 2, standby router priorities should be 90, all active router configs should come first before standby in both switches. for example : inter vlan 10, ip address 172.18.1.2 255.255.255.128, standby version 2, standby 10 ip 172.18.1.1, standby 10 priority 110, standby 10 preempt


**⚙️ SVR-DSW1 Configuration**

! STP CONFIGURATION
spanning-tree vlan 10,20,30,40 priority 24576
spanning-tree vlan 50,60,70,80 priority 28672

! VLAN INTERFACES + HSRP CONFIG (Active for 10-40)
interface Vlan10
 ip address 172.18.1.2 255.255.255.128
 standby version 2
 standby 10 ip 172.18.1.1
 standby 10 priority 110
 standby 10 preempt

interface Vlan20
 ip address 172.18.2.2 255.255.255.0
 standby version 2
 standby 20 ip 172.18.2.1
 standby 20 priority 110
 standby 20 preempt

interface Vlan30
 ip address 172.18.3.2 255.255.255.0
 standby version 2
 standby 30 ip 172.18.3.1
 standby 30 priority 110
 standby 30 preempt

interface Vlan40
 ip address 172.18.4.2 255.255.255.0
 standby version 2
 standby 40 ip 172.18.4.1
 standby 40 priority 110
 standby 40 preempt

! HSRP CONFIG (Standby for 50-80)
interface Vlan50
 ip address 172.18.5.2 255.255.255.0
 standby version 2
 standby 50 ip 172.18.5.1
 standby 50 priority 90

interface Vlan60
 ip address 172.18.6.2 255.255.255.248
 standby version 2
 standby 60 ip 172.18.6.1
 standby 60 priority 90


interface Vlan70
 ip address 172.18.7.2 255.255.255.0
 standby version 2
 standby 70 ip 172.18.7.1
 standby 70 priority 90

interface Vlan80
 ip address 172.18.8.2 255.255.255.0
 standby version 2
 standby 80 ip 172.18.8.1
 standby 80 priority 90


**⚙️ SVR-DSW2 Configuration**

! STP CONFIGURATION
spanning-tree vlan 50,60,70,80 priority 24576
spanning-tree vlan 10,20,30,40 priority 28672

! VLAN INTERFACES + HSRP CONFIG (Active for 50-80)
interface Vlan50
 ip address 172.18.5.3 255.255.255.0
 standby version 2
 standby 50 ip 172.18.5.1
 standby 50 priority 110
 standby 50 preempt

interface Vlan60
 ip address 172.18.6.3 255.255.255.248
 standby version 2
 standby 60 ip 172.18.6.1
 standby 60 priority 110
 standby 60 preempt


interface Vlan70
 ip address 172.18.7.3 255.255.255.0
 standby version 2
 standby 70 ip 172.18.7.1
 standby 70 priority 110
 standby 70 preempt

interface Vlan80
 ip address 172.18.8.3 255.255.255.0
 standby version 2
 standby 80 ip 172.18.8.1
 standby 80 priority 110
 standby 80 preempt

! HSRP CONFIG (Standby for 10-40)
interface Vlan10
 ip address 172.18.1.3 255.255.255.128
 standby version 2
 standby 10 ip 172.18.1.1
 standby 10 priority 90

interface Vlan20
 ip address 172.18.2.3 255.255.255.0
 standby version 2
 standby 20 ip 172.18.2.1
 standby 20 priority 90

interface Vlan30
 ip address 172.18.3.3 255.255.255.0
 standby version 2
 standby 30 ip 172.18.3.1
 standby 30 priority 90

interface Vlan40
 ip address 172.18.4.3 255.255.255.0
 standby version 2
 standby 40 ip 172.18.4.1
 standby 40 priority 90



Now, let's do ospf for the server side to the Edge routers

using the following addressing table and info, config ospf routing for these device networks. Ensure all vlan int and loopbacks are passive on all devices, the networks btw HQ routers and SVR-Cores are in area 0 while other networks are in area 2, loobpack addresses should be router ids






**SVR-CORE1**

router ospf 10
 router-id 1.2.2.1

 network 172.16.0.76 0.0.0.3 area 0
 network 172.16.0.60 0.0.0.3 area 0
 network 172.16.0.100 0.0.0.3 area 2
 network 172.16.0.104 0.0.0.3 area 2
 network 172.16.0.5 0.0.0.0 area 2

 passive-interface default
 no passive-interface GigabitEthernet1/0/1
 no passive-interface GigabitEthernet1/1/3
 no passive-interface GigabitEthernet1/0/4
 no passive-interface GigabitEthernet1/0/5


  int r g1/1/3,g1/0/1
 ip ospf network point-to-point 

**SVR-CORE2**

router ospf 10
 router-id 1.2.2.2

 network 172.16.0.64 0.0.0.3 area 0
 network 172.16.0.80 0.0.0.3 area 0
 network 172.16.0.108 0.0.0.3 area 2
 network 172.16.0.112 0.0.0.3 area 2
 network 172.16.0.6 0.0.0.0 area 2

 passive-interface default
 no passive-interface GigabitEthernet1/0/1
 no passive-interface GigabitEthernet1/1/3
 no passive-interface GigabitEthernet1/0/4
 no passive-interface GigabitEthernet1/0/5

  int r g1/1/3,g1/0/1
 ip ospf network point-to-point 


**SVR-DSW1**

router ospf 10
 router-id 1.2.3.1

 network 172.16.0.100 0.0.0.3 area 2
 network 172.16.0.112 0.0.0.3 area 2
 network 172.16.0.9 0.0.0.0 area 2

 network 172.16.0.100 0.0.0.3 area 2
 network 172.16.0.112 0.0.0.3 area 2
 network 172.16.0.9 0.0.0.0 area 2
 network 172.18.1.0 0.0.0.127 area 2  
 network 172.18.2.0 0.0.0.255 area 2   
 network 172.18.3.0 0.0.0.255 area 2  
 network 172.18.4.0 0.0.0.255 area 2   
 network 172.18.5.0 0.0.0.255 area 2  
 network 172.18.6.0 0.0.0.255 area 2   
 network 172.18.7.0 0.0.0.255 area 2 
 network 172.18.8.0 0.0.0.255 area 2   


 passive-interface default
 no passive-interface GigabitEthernet1/0/4
 no passive-interface GigabitEthernet1/0/5

**SVR-DSW2**


router ospf 10
 router-id 1.2.3.2

 network 172.16.0.104 0.0.0.3 area 2
 network 172.16.0.108 0.0.0.3 area 2
 network 172.16.0.10 0.0.0.0 area 2

network 172.18.1.0 0.0.0.127 area 2   
 network 172.18.2.0 0.0.0.255 area 2   
 network 172.18.4.0 0.0.0.255 area 2  
 network 172.18.5.0 0.0.0.255 area 2  
 network 172.18.6.0 0.0.0.255 area 2  
 network 172.18.7.0 0.0.0.255 area 2   
 network 172.18.8.0 0.0.0.255 area 2  

 passive-interface default
 no passive-interface GigabitEthernet1/0/4
 no passive-interface GigabitEthernet1/0/5







Symmetric Routing Configuration

I made significant changes to the IP addressing and OSPF routing in this lab. The main goal was to eliminate asymmetric routing between the ABRs and ASBRs. This is important to:
	•	Ensure consistent routing paths
	•	Improve network security
	•	Support effective edge router redundancy
	•	Simplify network management

To achieve this, I adjusted the OSPF interface costs between the ABRs and ASBRs, forcing OSPF to prefer specific paths for forwarding packets. In this topology:
	•	HQ-R1 is the preferred path
	•	HQ-R2 acts as the backup in case of a failover

⸻

WLC Configuration

I won’t lie — setting up the WLC took a lot of time, mainly due to Packet Tracer limitations. But I got it working!
	•	There’s one WLC managing lightweight APs across HQM, SVR, and eventually, two branches (branches not yet connected).
	•	SVR has one AP providing a single WLAN.
	•	HQM has two APs providing both Staff and Guest WiFi.

So far, everything works as expected in HQM and SVR.

⸻

Web and DNS Servers

Configured dedicated Web and DNS servers — straightforward setup and both servers are responding properly.

⸻

VTP Configuration

I set up VTP (VLAN Trunking Protocol) right from the start (probably forgot to document it earlier). Trust me, manually creating VLANs on 10 Layer 2 switches? Not the vibe.

If you’re unfamiliar with VTP:
It’s a Cisco proprietary protocol that helps distribute VLAN information across switches in the same domain using a server-client model. One switch acts as the VTP server, and all others as clients.

Basic VTP Setup:

On the Server:

vtp version 2  
vtp mode server  
vtp domain moonboy.com

On the Server:

vtp version 2  
vtp mode client

To verify:
show vtp status

Important Note:
Ensure all trunk links between switches have matching native VLANs and allowed VLANs. If not, you don gbezome — and if you don’t know what that means… otilo.


now let the branches be reachable using BDG Protocol

# 🌐 BGP Configuration for Multi-Branch Network

This configuration outlines a Border Gateway Protocol (BGP) setup involving **2 ISP routers** and **4 edge routers** at branch locations. The goal is to establish external BGP (eBGP) peering between all edge routers and both ISP routers while **advertising only necessary LAN subnets** to reduce routing overhead.

---

## 🖧 Router Roles and BGP AS Numbers

| Device | Role        | BGP ASN | Router ID |
| ------ | ----------- | ------- | --------- |
| ISP-1  | ISP Router  | 10      | 1.1.1.1   |
| ISP-2  | ISP Router  | 20      | 2.2.2.2   |
| HQ-R1  | Edge Router | 30      | 3.3.3.3   |
| HQ-R2  | Edge Router | 40      | 4.4.4.4   |
| VI-BR  | Edge Router | 50      | 5.5.5.5   |
| IKJ-BR | Edge Router | 60      | 6.6.6.6   |

---

## 🔗 IP Addressing Scheme

### Between ISPs

* ISP-1: `50.50.50.1`
* ISP-2: `50.50.50.2`

### HQ-R1 to ISPs

* To ISP-1: `20.20.20.2` ←→ `20.20.20.1`
* To ISP-2: `30.30.30.2` ←→ `30.30.30.1`

### HQ-R2 to ISPs

* To ISP-1: `20.20.20.6` ←→ `20.20.20.5`
* To ISP-2: `30.30.30.6` ←→ `30.30.30.5`

### IKJ-BR to ISPs

* To ISP-1: `20.20.20.10` ←→ `20.20.20.9`
* To ISP-2: `30.30.30.10` ←→ `30.30.30.9`

### VI-BR to ISPs

* To ISP-1: `20.20.20.14` ←→ `20.20.20.13`
* To ISP-2: `30.30.30.14` ←→ `30.30.30.13`

---

## 🔧 BGP Configurations

### ISP-1 (AS 10)

```bash
router bgp 10
 bgp router-id 1.1.1.1
 network 20.20.20.0 mask 255.255.255.224
 network 50.50.50.0 mask 255.255.255.252
 neighbor 20.20.20.2 remote-as 30
 neighbor 20.20.20.6 remote-as 40
 neighbor 20.20.20.10 remote-as 60
 neighbor 20.20.20.14 remote-as 50
```

### ISP-2 (AS 20)

```bash
router bgp 20
 bgp router-id 2.2.2.2
 network 30.30.30.0 mask 255.255.255.224
 network 50.50.50.0 mask 255.255.255.252
 neighbor 30.30.30.2 remote-as 30
 neighbor 30.30.30.6 remote-as 40
 neighbor 30.30.30.10 remote-as 60
 neighbor 30.30.30.14 remote-as 50
```

### HQ-R1 (AS 30)

```bash
router bgp 30
 bgp router-id 3.3.3.3
 neighbor 20.20.20.1 remote-as 10
 neighbor 30.30.30.1 remote-as 20
```

### HQ-R2 (AS 40)

```bash
router bgp 40
 bgp router-id 4.4.4.4
 neighbor 20.20.20.5 remote-as 10
 neighbor 30.30.30.5 remote-as 20
```

### IKJ-BR (AS 60)

```bash
router bgp 60
 bgp router-id 6.6.6.6
 neighbor 20.20.20.9 remote-as 10
 neighbor 30.30.30.9 remote-as 20
```

### VI-BR (AS 50)

```bash
router bgp 50
 bgp router-id 5.5.5.5
 neighbor 20.20.20.13 remote-as 10
 neighbor 30.30.30.13 remote-as 20
```
