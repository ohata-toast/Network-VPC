## Network > VPC > 摘要

VPC(Virtual Private Cloud)是一个在逻辑上彻底隔离的虚拟网络，在VPC中提供TOAST资源的运行。
每个VPC既可以构成独立的子网、路由表和网关，也可以相互控制。

VPC可以在控制台中进行创建和配置。VPC可以提供需要外部访问的网络服务，也可以实现屏蔽其他网络的数据库或应用的网络服务；同时还可以通过安全组的方式进行访问控制。

还可通过TCC1托管和连接功能的支持，享受更优质更值得信赖的云托管服务。




###主要功能

TOAST VPC支持公共网络或数据中心的内部托管连接，或通过网关进行外部连接。

*VPC的可用IP地址范围为所有私有IP地址。VPC中的每个子网，都可以在可用IP地址范围内自由创建。

* VPC的各个子网之间可通过不同的方法来连接。某些特定的子网，例如有外部网络服务的子网，有数据库需要隔离的子网，又有某些支持使用其他NAT网关的子网。

* 您可以使用路由表来配置各个符合服务的网络。以服务类别来划分子网，或以特定网络服务划分子网，也可以根据自己的需求来修改路由表。

* 可以使用弹性IP直接从网络上访问实例。

* 可以使用安全组对实例进行访问控制。

* 可以使用peering连接功能来实现不同VPC网络之间的访问。
如果需要网络完全隔离，可以使用多个VPC来分别提供服务，如果个别VPC网络间需要访问，可以使用peering连接功能。

* 一个实例可以绑定多个接口。如果一个实例需要访问多个相互隔离的子网中的服务时，可以通过在该实例上绑定多个接口的方式来实现相互通讯。

* VPC与其它TOAST服务的连接，将以隐藏的形式来提供。

<br>
### 用语

#### Subnet（子网）
Subnetwork，是在IP网络中细分为更小单位的IP地址区域。<br>
参考：https://en.wikipedia.org/wiki/Subnetwork

#### Internet Gateway（NAT网关）
通常被称为网关（gateway）,意思是由子网配置的网络连接到外部网络的路径。<br>
参考: https://en.wikipedia.org/wiki/Gateway_(telecommunications)#Internet_gateway

#### Routing Table（路由表）
由CIDR notation所定义的路由，可以指定到达目标地址的下一跳设备。<br>
参考：https://en.wikipedia.org/wiki/Routing_table

#### Private Network（私有网络）
根据网络地址分类，私有网络是使用在内部网络的地址，无法在公网上通讯。<br>
参考：https://en.wikipedia.org/wiki/Private_network

#### Peering
用于连接两个不同的VPC网络。

#### Interface（接口）
用于将实例连接到网络的设备成为接口。
