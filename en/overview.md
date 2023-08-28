## Network > VPC > Overview

Virtual Private Cloud (VPC) supports functions to operate NHN Cloud recourses in a logically isolated virtual network. Each VPC can configure completely standalone subnets, routing tables, and gateways, and control each of them. 
User can simply define VPC configuration in a console: connect services allowing internet access, or run database or application from a closed subnet. Each element can be protected by using security group. 
In addition, as connection with TCC1 hosting is available, VPC can enjoy advantages of both reliable hosting and the cloud service. 


### Main Features 

NHN Cloud VPC supports hosting connection within a public network or a data center, or external connection through an internet gateway. 

* Designate the range of a whole private IP address. Each subnet of VPC can be freely created within its range of IP address.   

* Use each subnet of VPC with different methods of connection: a subnet supports the web service allowing external access, and another supports closed database, while a third subnet supports connection of divided networks through another internet gateway.   

* Configure a network corresponding to each service, by using routing table. 
For subnets divided into each service, or a particular network service, modify the routing table as you want. 

* Directly access instances from the internet, via floating IP. 

* Control access to instances by using security group. 

* Apply a number of interfaces for a particular instance. To provide services for many subnets, depending on the purpose of an instance, the instance can be applied with an interface of many subnets.  

* VPC is provided while connection with other NHN Cloud services is hidden.

<br>
### Glossary

Terms  | Written Form          | Description                                                                                                                               
------------- |-------------|----------------------------------------------------------------------------------------------------------------------------------
Subnet  | Subnet         | Refers to a subnetwork, which is a subdivided IP address range within the IP network.<br><https://en.wikipedia.org/wiki/Subnetwork>                            
Routing Table | Routing Table | A table describing a transfer path defined by CIDR notation. By using destination addresses, you can specify the devices which the traffic is forwarded to or bypass.<br><https://en.wikipedia.org/wiki/Routing_table> 
Private Network| Private Network | Refers to a network configured with an IP address range that defines a private network in the structure of an internet address. The traffic is not delivered in a public network.<br><https://en.wikipedia.org/wiki/Private_network>   
Interface | Interface | A network interface refers to a device to connect an instance to a network.

