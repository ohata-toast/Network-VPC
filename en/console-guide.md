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

* Excessively transferred 'Broadcast, Multicast, Unknown Unicast' may be blocked without notice.

* You can specify whether to enable the **Private IP DNS** feature.
	* The feature is only available within the same VPC.
	* When you enable the **Private IP DNS** feature, you must reset the nameservers of the instances in the VPC to the private DNS ports.
  * The feature is only available within the same VPC.
  * When enabling the Private IP DNS feature, you must reset the nameservers of the instances in the VPC to the private DNS ports.
    * When you restart the instance, the nameservers for the instance are set to your private DNS port IP.
    * To change the nameservers without restarting, you must connect to the instance and point the nameservers to your private DNS port IPs. Follow the instructions for setting up nameservers for your operating system to make the change.

> [Note] Private DNS port
> * When you set up private IP DNS or connect VPC to the Zone for the first time, a private DNS port is created within the VPC.
> * For VPCs that are disconnected to all Zones without setting up private IP DNS, the private DNS port is deleted.
> * For VPCs that are disconnected and reconnected to all Zones without setting up private IP DNS, the private DNS port may be changed.
> * If a private DNS port is created/changed/deleted, the nameserver must be reset to reflect the changes.
>   * Restarting instances resets nameservers.
>   * To change the nameservers without restarting, you need to connect to the instance and change the nameservers. Follow the instructions for setting up nameservers for your operating system to make the change.
> * If you have restarted an instance for the application of private DNS port, you do not need to restart the instance if you subsequently connect another zone to that VPC or change the private IP DNS settings.
    
## Subnet

To attach resources to a virtual network (VPC) and give them IPs, you need to create one or more subnets. 

Subnets manage IP allocation pools and operate DHCP servers. Subnets assign available IPs to resources that need IP assignments, and register them with the DHCP server so that they can obtain the assigned IPs through DHCP.

By default, resources within all subnets of the same VPC can be accessed through the routing table without any configuration. Unlike VPCs, they are not fully isolated, so you may need to control access using security groups, ACLs, etc.

> [Note]
> Resources on subnets connected to different routing tables are also accessible to each other. 

When you select a subnet from the subnet list, you'll see the following information at the bottom of the list

### Subnet Basic Information
You can see basic information such as the subnet name, UUID, CIDR, and creation date, as well as the subnet's gateway IP and the number of available IPs that can be assigned to resources.
> [Note]
> The number of free IPs on a subnet is the number of IPs already assigned to resources on the subnet, excluding the IPs below.
> For example, if your subnet has a CIDR of `192.168.0.0/24`, you can use the
> * 192.168.0.0 - Network address
> * 192.168.0.1 - Gateway address. When a subnet is created, it is automatically associated with the **default routing table** included in VPC where the first IP is assigned as the gateway address.
> * 192.168.0.2 - DHCP Server address or IP for SNAT that connects to the Internet gateway
> * 192.168.0.3 - DHCP Server address or IP for SNAT that connects to an Internet gateway
> * 192.168.0.255 - Broadcast address
>
> The IP for the SNAT that connects to the Internet gateway is assigned when the routing table to which the subnet is connected has an Internet gateway connection, and might be assigned an IP other than `192.168.0.3`, depending on when the routing table and the Internet gateway were connected.

### Subnet Connection
A list of resources that have been assigned IPs on the subnet. You can see the resource's type, ID, assigned IP, and if the resource has a floating IP associated with it, its floating IP.

### Subnet Static Route
By using a subnet's **Static Route** setting, it is possible to pass the routing rules that instances in the subnet will set in the routing table within the instances at boot time.

* The routing rules registered in **Static Route** are sent by being included in the ‘classless-static-routes’ option of the response to the DHCP request requested by the instance. The DHCP client running in the instance registers the content of this option in the routing table.

* The method of reflecting the ‘classless-static-routes’ option varies depending on the OS type, distribution, or DHCP client version running on the instance. However, it is generally reflected when the DHCP client is first started, such as instance booting, and is not reflected when renewing the DHCP lease. Therefore, when you edit a subnet's **Static Route**, the change may not take effect immediately on the running instances, so it is recommended to reboot the running target instances if possible.

* A **Static Route** consists of the destination CIDR of the packet to be routed and the gateway information to forward the target packet to.<br>If you create a static route with a CIDR of "0.0.0.0/0", you can change the default gateway of the instance to an IP other than the gateway of the subnet.<br>The gateway is entered as text as opposed to a **Route** in the routing table, and you can also specify an IP that is not yet assigned within the subnet.

### Create Subnet
Enter the following information to create a subnet

* Name: Enter a name for the subnet.
* VPC: Select the VPC in which you want to create the subnet.
* IP version: Select the IP version to use on the subnet. Currently, only IPv4 is supported.
* CIDR: Enter the address range for the subnet. 
	* The address range of the subnet must be included in the VPC address range, with a minimum range of 28 bits.
	* The address range must not overlap with other subnets within the same VPC.
	* You can't change the scope after the subnet is created.

> [Note]
> You can create a maximum of 10 subnets per VPC.
> If you need to create more than 11 subnets within a single VPC, please contact our [Customer Center](https://www.nhncloud.com/kr/support/inquiry). 

### Modify Subnet
You can rename the subnet. 

### Delete Subnet
Deletes the selected subnet. You can delete a subnet only if it does not contain any resources, such as instances or load balancers, that have IP assignments from the subnet. 

### Attach Routing table
'Explicitly' attach the selected subnet to the selected routing table.

When you create a subnet, it is 'implicitly' attached to the VPC's default routing table, which you can 'explicitly' attach to the default routing table or to another routing table using the Connect Routing Table feature.

### Detach Routing Table
If the subnet and routing table are 'explicitly' attached, detach them.

Subnets that are detached from the routing table are 'implicitly' attached back to the VPC's **default routing table**.

> [Note]
> * Implicitly attached: Subnets are automatically attached to the **default routing table**. If you specify a different routing table as the default routing table for the VPC, subnets that were implicitly attached to the existing default routing table are detached and automatically attached to the newly specified default routing table.
   In this case, the **Subnet Basic Information > Routing Table** entry will show as **(Routing Table of Default Connection)**.
> * Explicitly attached: Subnets are explicitly attached to a routing table, using the **Attach Routing Table** feature. All attachments are explicit unless you attach to the default routing table, in which case specifying the default routing table changes the implicit attachment state to explicit. In this case, even if you change the default routing table, the subnet retains its association with the existing routing table.

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
