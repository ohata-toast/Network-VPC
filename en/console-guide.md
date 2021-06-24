## Network > VPC > Console Guide

This document describes what is required to deal with VPCs in the console.

## VPC

AS a VPC may have many subnets, a sufficiently large network must be configured when divided subnets are used. VPC network can be described by using [CIDR Notation](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).  All VPCs must be located in the three address areas as below, to be able to configure a [Private Network](https://en.wikipedia.org/wiki/Private_network) and link-local addresses cannot be used. Furthermore, the network area must be larger than 24bit-256 at the least. 

### Private Network 

RFC1918 | IP Address Area | Available Number of Addresses 
-------- | ---------- | -----------------
24bit block | 10.0.0.0/8 | 16,777,216
20bit block | 172.16.0.0/12 | 1,047,576
16bit block | 192.168.0.0/16 | 65,536

### Link-Local Addresses 

Cannot use 65,536 IP addresses that are included to 169.254.0.0/16. 

### Examples 

Example | Availability 
-------- | ---------- 
10.0.0.0/8 | Available. 
10.0.0.0/16 | Available. 
10.0.0.0/24 | Available. 
10.0.0.0/28 | Unavailable. Range is too short. 
172.16.0.0/16 | Available. 
172.16.0.0/8 | Unavailable. Out of available range. 
192.168.0.0/16 | Available. Specified as default range. 
192.168.0.0/24 | Available. 
192.253.0.0/24 | Unavailable. Out of available range. 

<br>


By using initial compute and network products, items as below are to be automatically configured. 

Item | Name | Summary 
-------- | ---------- | --------------
VPC | Default Network | Creates one VPC within the range of 192.168.0.0/16. 
Subnet | Default Network | Creates one subnet within the range of 192.168.0.0/24. 
Routing Table | vpc-[id] | Creates one routing table named after a part of VPC ID. 
Internet Gateway | ig-[id] | Creates one internet gateway named after a part of routing table ID. 
Security Group | default | Creates a security group named default. 

To add VPC, not as an initial configuration, configure items as below: 

Item | Name | Summary 
-------- | ---------- | --------------
VPC | As specified | Creates one VPC within the specified range. 
Subnet | - | Not created. 
Routing Table | vpc-[id] | Creates one routing table named after a part of VPC ID. 
Internet Gateway | - | Not created: create additionally and connect. 
Security Group | - | Not created additionally. 

Here is the quota for VPC and each item.  

Item | Max 
-------- | ---------- 
VPC | 3
Subnet | 10 per VPC 
Internet Gateway | 3 
Floating IP | Unlimited 
Routing Table | 10 per VPC 
Route | 10 per Routing Table 
Peering | Unlimited 
NAT gateway | 3


> [Note]
VPCs can be deleted only when subnets are deleted altogether, and in such case, delete along with subnets, routing tables, and internet gateways. 

* VPC is safe from traffic as it is completed isolated from other VPCs .

* Direct access of VPC from the internet is unavailable as it is a private network.  

* All elements within VPC cannot use VLAN. 

* For those traffic beyond  a region, local communication is not provided.  

* All instances within a VPC cannot access the internet without internet gateway. 

* Excessively transferred "Broadcast, Multicast, Unknown Unicast" may be blocked without prior notice. 


## Subnets 

A  VPC can be subdivided into subnets that are composed of many networks. However, a subnet must be included within the range of a VPC address, with its length the same or shorter.  For example, in the case of 192.168.0.0/16, a total of 65536 IP addresses are available between 192.168.0.0 and 192.168.255.255. In addition, the smallest subnet is 28 bits and any configuration cannot be smaller than that. Subnets also adopt CIDR display, just like VPC.  

When a subnet is created, its gateway IP address is automatically designated and it cannot be modified. It is also automatically registered to a routing table included to the VPC. 

> [Note]
To delete a subnet, it should be empty with no instances or load balancers included. Also, make sure that the routing table which the subnet is connected to does not have  routes to that subnet.

* Assigns one IP address to a newly-created instance from a designated subnet (called a fixed IP).

* Applies the IP address to the instance when it is booted, via DHCP.  

* Cannot modify the address range of a subnet. 

* Cannot redundantly create or overlap the range for different subnets within a same VPC.

* The range of subnets may be redundant or wrapped over for different VPCs. 

* MAC addresses that are not assigned to an instance may be blocked from the network; VPN service running on instances may not operate. 

* When many subnets are connected to an instance, an appropriate routing setting is required within OS of the instance. 

* Two subnets within a same VPC are not completely isolated; apply security groups to protect instances. 

* Subnets support local communication through different areas of availability. Local communication shall not be charged.  


## Internet Gateway

An internet gateway can be connected to a routing table. VPCs comprised of private networks cannot be connected externally, and may access the internet through internet gateway. Each instance, to be connected to an internet, must set "default gateway" as the gateway address of the subnet, and NHN Cloud does the job automatically. To create an internet gateway, an external network is required and NHN Cloud is operating only one "public_network" as of now.  

* An internet gateway address is automatically assigned when an instance is created or VPC requires an internet access, and it cannot be modified. 

* Cannot access an instance with an internet gateway address. 

* Blocks all traffic inflow via internet gateway address.

* Charges by usage, when an instance connected to the internet triggers traffic to the internet direction.

* Do not charge for local communication between instances. 

### Guide for Restarting Internet Gateways for Server Maintenance 

NHN Cloud updates software of the Internet gateway server on a regular basis to enhance security and stability of its infrastructure services. 

Internet gateways that are running on a target server for maintenance must be restarted and migrated to the server which is completed with maintenance.  

To restart Internet gateways, use the **! Restart** button which is created next to each Internet gateway name. 

Go to the project where your Internet gateway specified as maintenance target is located. 

1. Any Internet gateway that has the **! Restart** button before its name requires maintenance. 
    ![ig-001](http://static.toastoven.net/prod_vpc/ConsoleGuide/ig_planned_migration_guide-en-001.png)
    Put the mouse cursor on the **! Restart** button to find maintenance schedule details. 
    ![ig-002](http://static.toastoven.net/prod_vpc/ConsoleGuide/ig_planned_migration_guide-en-002.png)

2. Select a target Internet gateway and click the **! Restart** button next to its name. 
    It is advised to perform maintenance during time when impact on service is limited, since instances of the Internet gateway are disconnected from the Internet until restarting is completed. 
    However, instances that are connected to floating IPs are not influenced by restarting of Internet gateways. 

3. Click **OK** onto the window asking of restarting Internet gateway. 
    ![ig-003](http://static.toastoven.net/prod_vpc/ConsoleGuide/ig_planned_migration_guide-en-003.png)

4. Wait until the Internet gateway status turns green and the **! Restart** button disappears.
    If the status does not change or the **! Restart** button remains, press 'Refresh'.  
    ![ig-004](http://static.toastoven.net/prod_vpc/ConsoleGuide/ig_planned_migration_guide-en-004.png)

The Internet gateway becomes inoperable while restarting is underway.
Unless restarting Internet gateway is normally completed, it shall be automatically reported to the administrator, and you'll be contacted by NHN Cloud.  


## NAT gateway

This feature is available only in the Pyeongchon region, Korea.
NAT gateway can be set as a gateway from the route setting of a routing table. If NAT gateway is set as the gateway for the routing table, the floating IP of NAT gateway can be switched to source IP to access the Internet.

| Item      | Description                                                         |
| --------|------------------------------------------------------------ |
| Name      | Can specify the name of NAT gateway. |
| VPC      | Specifies the VPC to create NAT gateway. |
| Subnet     | Specifies the subnets connected to a routing table that is connected to the Internet gateway among the subnets of the selected VPC. NAT gateway cannot be created with a subnet that does not satisfy the conditions. |
| Floating IP  | Specifies the floating IP to be assigned to NAT gateway. This IP is converted to the source IP when connecting to the Internet. |
| Description      | Can add the description for NAT gateway. |

* The VPC, subnet, and floating IP of an NAT gateway cannot be changed.

* The floating IP configured by NAT gateway cannot be disconnected. It is automatically disconnected when the NAT gateway is deleted.

* Instances cannot be accessed with the NAT gateway address.

* The inbound traffic to the NAT gateway address is entirely blocked.

* A single NAT gateway can be specified as a gateway in multiple routing tables in the same VPC.

* Subnets specified when NAT gateway is configured and routing tables connected to a different subnet can specify NAT gateway as its gateway.

* Network ACL is applied.

* If the gateway is set to 'NAT gateway’ for the target CIDR to route without IP Prefix 0 (/0) in the Route Settings, 'NAT Gateway' will be used to communicate even if floating IP is set for the instance.



## Routing Table 

A routing table is created along with VPC, and is also deleted along with a deletion of VPC. Many of them can be created within a VPC, and they may be deleted if not a default routing table. Subnets must be connected to at least one routing table, and multiple routing tables cannot share an internet gateway.   

When a list of routing table is designated, summary information is displayed on the screen of details, and more routes may be added by using "Routes". 

> [Note]
Adding more routes is available, only when available areas within VPC are designated. Otherwise, message will show failure. 

* Gateways of subnets included to a routing table are automatically added.  

* “Default routing table” cannot be deleted. It shall be deleted along with VPC on its deletion.

* Gateways of subnets and internet cannot be deleted from the list of routers. 

* Disconnecting a routing table from the internet gateway will disconnect the internet. 



## Peering 

Peering refers to connecting two different VPCs. In general, VPCs cannot communicate as their network areas are different, and may be linked via floating IPs but charged further depending on the network usage. Hence, peering is provided to communicate two VPCs 

> [Note] Peering connects two different VPCs; does not support a connection to a third VPC through another VPC. That is, for A <-> B <-> C, A and C cannot be linked.  

* Cannot use overlapped IP address areas of two VPCs.<br>When IP address areas of two VPCs to create peering are redundant, peering fails. 
* Size of an IP address area does not matter and communication with unlinked subnets to "default routing table" is unavailable. 
  * [Pyeongchon region, Korea] Communications become available only when a separate route is configured from the routing table of both VPCs after peering is created.
    * Route can be added by entering the IP address of the other side in the "Target CIDR" of a route and selecting the item named "PEERING" from the "Gateway" list.
    * Communications are available only through the subnet connected to the routing table that added a route.
* Peering is charged from the moment it is created. 
* There's no quota limits for a peering, but a subnet quota is required. 
  * [Pyeongchon region, Korea] It now uses each IP from two peered VPCs instead of subnet quota.



## Co-location gateway

The co-location gateway is a feature used to connect customer network, which is provided as a hybrid service by NHN Cloud. This feature is available only in the Pyeongchon region, Korea. If a hybrid service is used in NHN Cloud, NHN Cloud Zone is provided. It can be used to directly connect to VPC by configuring the co-location gateway.
* A single VPC is connected to a single NHN Cloud Zone one-on-one.
* The fee is charged the moment the VPC is connected.


