## Network > VPC > Overview

Virtual Private Cloud (VPC) supports functions to operate TOAST recourses in a logically isolated virtual network. Each VPC can configure completely standalone subnets, routing tables, and gateways, and control each of them. 

User can simply define VPC configuration in a console: connect services allowing internet access, or run database or application from a closed subnet. Each element can be protected by using security group. 

In addition, as connection with TCC1 hosting is available, VPC can enjoy advantages of both reliable hosting and the cloud service. 




### Main Features 

TOAST VPC supports hosting connection within a public network or a data center, or external connection through an internet gateway. 

* Designate the range of a whole private IP address. Each subnet of VPC can be freely created within its range of IP address.   

* Use each subnet of VPC with different methods of connection: a subnet supports the web service allowing external access, and another supports closed database, while a third subnet supports connection of divided networks through another internet gateway.   

* Configure a network corresponding to each service, by using routing table. For subnets divided into each service, or a particular network service, modify the routing table as you want. 

* Directly access instances from the internet, via floating IP. 

* Control access to instances by using security group. 

* Connect different VPCs via peering. To allow complete isolation, use multiple VPCs and provide peering for designated connection. 

* Apply a number of interfaces for a particular instance. To provide services for many subnets, depending on the purpose of an instance, the instance can be applied with an interface of many subnets.  

* VPC is provided while connection with other TOAST services is hidden.  

<br>
### Glossary

#### Subnet

Refers to a subnetwork, which is a subdivided IP address area within the IP network.<br>https://en.wikipedia.org/wiki/Subnetwork

#### Internet Gateway

Generally known as Gateway, it refers to a pathway of a network configured by a subnet to outside.<br>https://en.wikipedia.org/wiki/Gateway_(telecommunications)#Internet_gateway

#### Routing Table

A table describing a transfer path as defined by CIDR notation: transferred devices may be designated or bypassed by destination address.<br>https://en.wikipedia.org/wiki/Routing_table

#### Private Network

Refers to a network marked as an IP area which defines a private network in the structure of an internet address: the traffic is not delivered in a public network. <br>https://en.wikipedia.org/wiki/Private_network>

#### Peering

Peering refers to connecting two different VPCs. 

#### Interface

A device to connect an instance to a network.

