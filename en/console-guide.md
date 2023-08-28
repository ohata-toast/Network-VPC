## Network > VPC > Console Guide

This document describes what you need to do when working with VPCs in the console.

## VPC

Since a VPC can have multiple subnets, a sufficiently large network must be configured when divided subnets are used. A VPC network can be described by using [CIDR notation](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing). All VPCs must be located in the three address ranges shown below, where a [private network](https://en.wikipedia.org/wiki/Private_network) can be configured, and link-local addresses cannot be used. In addition, you must specify a network area that is larger than 24bit-256.

### Private Network 

RFC1918 | IP Address Range | Number of Available Addresses 
-------- | ---------- | -----------------
24bit block | 10.0.0.0/8 | 16,777,216
20bit block | 172.16.0.0/12 | 1,047,576
16bit block | 192.168.0.0/16 | 65,536

### Link-Local Addresses 

You cannot use 65,536 IP addresses that are included in 169.254.0.0/16. 

### Examples 

Example | Availability 
-------- | ---------- 
10.0.0.0/8 | Available. 
10.0.0.0/16 | Available. 
10.0.0.0/24 | Available. 
10.0.0.0/28 | Unavailable. Range is too small. 
172.16.0.0/16 | Available. 
172.16.0.0/8 | Unavailable. Out of available range. 
192.168.0.0/16 | Available. Specified as the default range. 
192.168.0.0/24 | Available. 
192.253.0.0/24 | Unavailable. Out of available range. 

<br>


When you use Compute and Network products for the first time, the following items are automatically configured.

Item | Name | Summary 
-------- | ---------- | --------------
VPC | Default Network | Creates one VPC within the range of 192.168.0.0/16. 
Subnet | Default Network | Creates one subnet within the range of 192.168.0.0/24. 
Routing Table | vpc-[id] | Creates one routing table named after a part of VPC ID. 
Internet Gateway | ig-[id] | Creates one internet gateway named after a part of routing table ID. 
Security Group | default | Creates a security group named default. 

When adding a VPC rather than performing the initial configuration, the following items are configured.

Item | Name | Summary 
-------- | ---------- | --------------
VPC | Specified name | Creates one VPC within the specified range. 
Subnet | - | Not created. 
Routing Table | vpc-[id] | Creates one routing table named after a part of VPC ID. 
Internet Gateway | - | Not created, so it must be created and connected separately.
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


> [Note]
VPCs can be deleted only when subnets are deleted altogether, and in such case, delete along with subnets, routing tables, and internet gateways. 

* VPC is safe from traffic because it is completely isolated from other VPCs.

* Direct access of VPC from the internet is unavailable as it is a private network.  

* All elements within VPC cannot use VLAN. 

* For those traffic beyond a region, local communication is not provided.  

* All instances within a VPC cannot access the internet without internet gateway. 

* Excessively transferred "Broadcast, Multicast, Unknown Unicast" may be blocked without prior notice.
    
## Subnets 

A VPC can be subdivided into subnets that are composed of multiple networks. However, a subnet must be included within the range of a VPC address, with its length the same or shorter. For example, in the case of 192.168.0.0/16, a total of 65536 IP addresses are available between 192.168.0.0 and 192.168.255.255. In addition, the smallest subnet is 28 bits and any configuration cannot be smaller than that. Subnets also adopt CIDR display, just like VPC.  

When a subnet is created, it is automatically associated with the default routing table included in the VPC. At this time, the gateway IP is assigned automatically.

> [Note]
To delete a subnet, it should be empty with no instances or load balancers included. Also, make sure that the routing table which the subnet is associated with does not have routes to that subnet.

* Assigns one IP address to a newly-created instance from a designated subnet (called a fixed IP).

* Applies the IP address to the instance when it is booted, via DHCP.  

* Cannot modify the address range of a subnet. 

* Cannot redundantly create or overlap the range for different subnets within a same VPC.

* The range of subnets may be redundant or wrapped over for different VPCs. 

* MAC addresses that are not assigned to an instance may be blocked from the network. Therefore, a VPN service running on instances may not work. 

* When multiple subnets are connected to an instance, an appropriate routing setting is required within OS of the instance. 

* Two subnets within a same VPC are not completely isolated. Apply security groups to protect instances. 

* Subnets support local communication through different areas of availability. Local communication shall not be charged.  

By using a subnet's "Static Route" setting, it is possible to pass the routing rules that instances in the subnet will set in the routing table within the instances at boot time.

* The routing rules registered in "Static Route" are sent by being included in the "classless-static-routes" option of the response to the DHCP request requested by the instance. The DHCP client running in the instance registers the content of this option in the routing table.

* The method of reflecting the "classless-static-routes" option varies depending on the OS type, distribution, or DHCP client version running on the instance. However, it is generally reflected when the DHCP client is first started, such as instance booting, and is not reflected when renewing the DHCP lease. Therefore, when you edit a subnet's "Static Route", the change may not take effect immediately on the running instances, so it is recommended to reboot the running target instances if possible.

* A "Static Route" consists of the destination CIDR of the packet to be routed and the gateway information to forward the target packet to.<br>If you create a static route with a CIDR of "0.0.0.0/0", you can change the default gateway of the instance to an IP other than the gateway of the subnet.<br>The gateway is entered as text as opposed to a "Route" in the routing table, and you can also specify an IP that is not yet assigned within the subnet.


## Routing Table 

A routing table is created along with VPC, and is also deleted along with a deletion of VPC. Multiple routing tables can be created within a VPC, and can be deleted explicitly if they are not the default routing table. Subnets must be associated with at least one routing table, and multiple routing tables cannot share an internet gateway.   

When a list of routing table is designated, summary information is displayed on the screen of details, and more routes may be added by using "Routes". 

> [Note]
Adding more routes is available, only when available areas within VPC are designated. Otherwise, message will show failure. 

* Gateways of subnets included to a routing table are automatically added.  

* “Default routing table” cannot be deleted. It shall be deleted along with VPC on its deletion.

* Gateways of subnets and internet cannot be deleted from the list of routers. 

* Disconnecting a routing table from the internet gateway will result in a loss of internet connection.

* You can set a gateway for a specific CIDR by creating a route in the routing table. Note that communication may be cut off if the wrong route is set.

A routing table can be created in either a "distributed virtual routing (DVR)" or a "centralized virtual routing (CVR)" method, depending on where it is created.

* Distributed virtual routing (DVR) method

    * A routing table is created for each hypervisor on which an instance in the subnet associated with the routing table is located. Routing tables are distributed evenly across the locations where they are used, providing reliability and high availability.

* Centralized virtual routing (CVR) method

    * One routing table is created at the center, and traffic from instances included in the subnet associated with the routing table is concentrated on one routing table.<br>This method is used when traffic is controlled through a single point, such as a gateway or firewall.

The DVR method is the default method provided by NHN Cloud. It is recommended to use the DVR method except for special circumstances because the method offers advantages of reliability, high availability, and traffic distribution.

It is also possible to change the routing table method. Note that, when changing the routing table method, external communication and inter-subnet communication will be cut off for about 1 minute until the reconfiguration of routing table is completed.
