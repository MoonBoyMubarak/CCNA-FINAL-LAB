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
 



! VLAN 10 - Mgmt
ip dhcp excluded-address 172.17.1.1 172.17.1.10
ip dhcp pool Mgmt
 network 172.17.1.0 255.255.255.128
 default-router 172.17.1.1

! VLAN 20 - Phones
ip dhcp excluded-address 172.17.2.1 172.17.2.10
ip dhcp pool Phones
 network 172.17.2.0 255.255.255.0
 default-router 172.17.2.1

! VLAN 30 - CustCARE
ip dhcp excluded-address 172.17.3.1 172.17.3.10
ip dhcp pool CustCARE
 network 172.17.3.0 255.255.255.0
 default-router 172.17.3.1

! VLAN 40 - Printers
ip dhcp excluded-address 172.17.4.1 172.17.4.10
ip dhcp pool Printers
 network 172.17.4.0 255.255.255.0
 default-router 172.17.4.1

! VLAN 50 - Sales
ip dhcp excluded-address 172.17.5.1 172.17.5.10
ip dhcp pool Sales
 network 172.17.5.0 255.255.255.0
 default-router 172.17.5.1

! VLAN 60 - Staff—Wi-fi
ip dhcp excluded-address 172.17.6.1 172.17.6.10
ip dhcp pool Staff-WiFi
 network 172.17.6.0 255.255.255.0
 default-router 172.17.6.1

! VLAN 70 - Guest—Wi-fi
ip dhcp excluded-address 172.17.7.1 172.17.7.10
ip dhcp pool Guest-WiFi
 network 172.17.7.0 255.255.255.0
 default-router 172.17.7.1

! VLAN 80 - Acctng
ip dhcp excluded-address 172.17.8.1 172.17.8.10
ip dhcp pool Acctng
 network 172.17.8.0 255.255.255.0
 default-router 172.17.8.1


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
 router-id 172.16.0.7
 network 172.16.0.84 0.0.0.3 area 1
 network 172.16.0.96 0.0.0.3 area 1
 network 172.16.0.7 0.0.0.0 area 1

 network 172.16.1.0 0.0.0.255 area 1
 network 172.16.2.0 0.0.0.255 area 1
 network 172.16.3.0 0.0.0.255 area 1
 network 172.16.4.0 0.0.0.255 area 1
 network 172.16.5.0 0.0.0.255 area 1
 network 172.16.6.0 0.0.0.255 area 1
 network 172.16.7.0 0.0.0.255 area 1
 network 172.16.8.0 0.0.0.255 area 1

 passive-interface default
 no passive-interface GigabitEthernet1/0/23
 no passive-interface GigabitEthernet1/0/24

#### HQ-DSW2 OSPF Configuration

router ospf 10
 router-id 172.16.0.8
 network 172.16.0.88 0.0.0.3 area 1
 network 172.16.0.92 0.0.0.3 area 1
 network 172.16.0.8 0.0.0.0 area 1

 network 172.16.1.0 0.0.0.255 area 1
 network 172.16.2.0 0.0.0.255 area 1
 network 172.16.3.0 0.0.0.255 area 1
 network 172.16.4.0 0.0.0.255 area 1
 network 172.16.5.0 0.0.0.255 area 1
 network 172.16.6.0 0.0.0.255 area 1
 network 172.16.7.0 0.0.0.255 area 1
 network 172.16.8.0 0.0.0.255 area 1

 passive-interface default
 no passive-interface GigabitEthernet1/0/23
 no passive-interface GigabitEthernet1/0/24


#### HQ-CORE1 OSPF Configuration

router ospf 10
 router-id 172.16.0.3

 network 172.16.0.68 0.0.0.3 area 0  
 network 172.16.0.52 0.0.0.3 area 0  
 network 172.16.0.84 0.0.0.3 area 1
 network 172.16.0.96 0.0.0.3 area 1
 network 172.16.0.116 0.0.0.3 area 1

 passive-interface Loopback0


#### HQ-CORE2 OSPF Configuration 
router ospf 10
 router-id 172.16.0.4

 network 172.16.0.56 0.0.0.3 area 0
 network 172.16.0.72 0.0.0.3 area 0
 network 172.16.0.92 0.0.0.3 area 1
 network 172.16.0.96 0.0.0.3 area 1
 network 172.16.0.116 0.0.0.3 area 1

 passive-interface Loopback0



 let's do ospf for HQ-R1 and HQ-R2 with networks we excluded to remain in area 0. HQ-R1 has 172.16.0.68/30 and .72/30 then HQ-R2 has networks 172.16.0.52/30 and .56/30

#### HQ-R1 OSPF Configuration

 router ospf 10
 router-id 172.16.0.1


 network 172.16.0.68 0.0.0.3 area 0
 network 172.16.0.72 0.0.0.3 area 0
 network 172.16.0.1 0.0.0.0 area 0   

 passive-interface Loopback0

#### HQ-R2 OSPF Configuration
router ospf 10
 router-id 172.16.0.2

 network 172.16.0.52 0.0.0.3 area 0
 network 172.16.0.56 0.0.0.3 area 0
 network 172.16.0.2 0.0.0.0 area 0  
 passive-interface Loopback0




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


! VLAN 20 - Phones
ip dhcp excluded-address 172.18.2.0 172.18.2.10
ip dhcp pool Phones
   network 172.18.2.0 255.255.255.0
   default-router 172.18.2.1
   

! VLAN 30 - IT-SUPPORT
ip dhcp excluded-address 172.18.3.0 172.18.3.10
ip dhcp pool IT-SUPPORT
   network 172.18.3.0 255.255.255.0
   default-router 172.18.3.1
   

! VLAN 40 - SVR-WIFI
ip dhcp excluded-address 172.18.4.0 172.18.4.10
ip dhcp pool SVR-WIFI
   network 172.18.4.0 255.255.255.0
   default-router 172.18.4.1
   

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
 ip address 172.18.6.2 255.255.255.0
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
 ip address 172.18.6.3 255.255.255.0
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


**HQ-R1**

router ospf 10
 router-id 172.16.0.1

 network 172.16.0.76 0.0.0.3 area 0
 network 172.16.0.80 0.0.0.3 area 0
 network 172.16.0.1 0.0.0.0 area 0

 passive-interface default
 no passive-interface GigabitEthernet0/0
 no passive-interface GigabitEthernet0/3/0

**HQ-R2**
router ospf 10
 router-id 172.16.0.2

 network 172.16.0.60 0.0.0.3 area 0
 network 172.16.0.64 0.0.0.3 area 0
 network 172.16.0.2 0.0.0.0 area 0

 passive-interface default
 no passive-interface GigabitEthernet0/3/0
 no passive-interface GigabitEthernet0/0

**SVR-CORE1**

router ospf 10
 router-id 172.16.0.5

 network 172.16.0.76 0.0.0.3 area 0
 network 172.16.0.60 0.0.0.3 area 0
 network 172.16.0.100 0.0.0.3 area 2
 network 172.16.0.104 0.0.0.3 area 2
 network 172.16.0.5 0.0.0.0 area 2

 passive-interface default
 no passive-interface GigabitEthernet1/0/1
 no passive-interface GigabitEthernet1/1/2
 no passive-interface GigabitEthernet1/0/4
 no passive-interface GigabitEthernet1/0/5

**SVR-CORE2**

router ospf 10
 router-id 172.16.0.6

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


**SVR-DSW1**

router ospf 10
 router-id 172.16.0.9

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
 router-id 172.16.0.10

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






