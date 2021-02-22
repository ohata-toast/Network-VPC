## Network > VPC > API v2 Guide 

To enable APIs, API endpoint and token are required. Prepare information required to enable an API, in reference of  [Preparing for APIs](/Compute/Compute/ko/identity-api/)

Use  `network`- type endpoint for VPC API. For more details, see `serviceCatalog` from response of token issuance. 

| Type | Region | Endpoint |
|---|---|---|
| Network | Korea (Pangyo)<br>Japan | https://kr1-api-network.infrastructure.cloud.toast.com<br>https://jp1-api-network.infrastructure.cloud.toast.com |

In API response, you may find fields that are not specified in the guide. Refrain from using them because such fields are only for the TOAST internal usage and might be changed without previous notice. 

## Network
### List Networks 
Return the list of available networks. 
```
GET /v2.0/networks
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| id | Query | UUID | - | Network ID to query |
| name | Query | String | - | Network name to query |
| provider:network_type | Query | Enum | - | Network type to query<br>Either`flat` or `vlan` |
| router:external | Query | Boolean | - | Whether network is externally connected |
| shared | Query | Boolean | - | Whether to share network to query |
| tenant_id | Query | String | - | Tenant ID to which network to query is included |
| sort_dir | Query | Enum | - | Sorting direction of network to query <br>Sort by the field specified by`sort_key`<br>Either **asc**, or **desc** |
| sort_key | Query | String | - | Sorting key of network to query <br>Sort in the direction as specified by`sort_dir` |
| fields | Query | String | - | Field name of network to query<br>e.g.) `fields=id&fields=name` |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| networks | Body | Array | List of network information objects |
| networks.status | Body | Enum | Network status <br>One of **ACTIVE**, **DOWN**, **BUILD**, and **ERROR** |
| networks.subnets | Body | Array | List of subnet IDs included to network |
| networks.name | Body | String | Network name |
| networks.router:external | Body | Boolean | Whether network is externally connected |
| networks.tenant_id | Body | String | Tenant ID |
| networks.admin_state_up | Body | Boolean | Status of admin control <br>`true`: Available<br>`false`: Unavailable |
| networks.mtu | Body | Integer | Maximum Transmission Unit |
| networks.shared | Body | Boolean | Whether to share network |
| networks.port_security_enabled | Body | Boolean | Whether network port is secured <br>Decide whether to enable security of port created in the network |
| networks.id | Body | String | Network ID |
| networks.name | Body | String | Network name |
| networks_links | Body | Array | Information object for pagination <br>Return when`limit` or `offset` is added<br>Includes paths indicating the next list |

<details><summary>Example</summary>
<p>


```json
{
  "networks": [
    {
      "status": "ACTIVE",
      "subnets": [
        "b83863ff-0355-4c73-8c10-0bdf66a69aab"
      ],
      "name": "Default Network",
      "router:external": false,
      "tenant_id": "6cdebe3eb0094910bc41f1d42ebe4cb7",
      "admin_state_up": true,
      "mtu": 0,
      "shared": false,
      "port_security_enabled": true,
      "id": "245ff686-4ca2-4176-a069-81013537ac3a"
    },
    {
      "name": "public_network",
      "id": "b04b1c31-f2e9-4ae0-a264-02b7d61ad618",
      "status": "ACTIVE",
      "shared": true,
      "subnets": [
        "6b3f7d6d-df61-4345-beb5-1621fd274659",
        "f22ae5cb-5e52-4704-9c31-83dc3826efb7"
      ],
      "admin_state_up": true,
      "port_security_enabled": true,
      "router:external": true,
      "tenant_id": "e873d250f2ca40b78e2c12cfbaaeb740",
      "mtu": 0
    }
  ]
}
```

</p>
</details>

---

## Subnet
### List Subnets
Return available list of subnets. 
```
GET /v2.0/subnets
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| id | Query | UUID | - | Subnet ID to query |
| name | Query | String | - | Subnet name to query |
| enable_dhcp | Query | Boolean | - | Whether to enable DHCP of subnet to query |
| network_id | Query | UUID | - | Network ID of subnet to query |
| cidr | Query | String | - | Subnet CIDR to query |
| shared | Query | Boolean | - | Whether to share subnet to query |
| sort_dir | Query | Enum | - | Sorting direction of subnet to query <br>Sort by the field specified by`sort_key`<br>Either **asc**, or **desc** |
| sort_key | Query | String | - | Sorting key of subnet to query <br>Sort in the direction as specified by `sort_dir` |
| fields | Query | String | - | Field name of subnet to query <br>e.g.) `fields=id&fields=name` |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| subnets | Body | Array | List of subnet information objects |
| subnets.name | Body | String | Subnet name |
| subnets.enable_dhcp | Body | Boolean | Whether to enable DHCP of subnet |
| subnets.network_id | Body | UUID | Network ID of subnet |
| subnets.tenant_id | Body | String | Tenant ID |
| subnets.dns_nameservers | Body | Array | List of DNS name servers connected to subnet |
| subnets.gateway_ip | Body | String | Gateway IP of subnet |
| subnets.ipv6_ra_mode | Body | Boolean | The router advertisement mode of IPv6 |
| subnets.allocation_pools | Body | Array | List of objects within subnet IP range |
| subnets.allocation_pools.start | Body | String | The starting IP address of subnet IP range |
| subnets.allocation_pools.end | Body | String | The last IP address of subnet IP range |
| subnets.host_routes | Body | Array | List of additional path information of subnet |
| subnets.host_routes.destination | Body | String | Destination<br>If the address of destination is `destination`<br>deliver to address specified as `nexthop` |
| subnets.host_routes.nexthop | Body | String | The next hop address |
| subnets.ip_version | Body | Integer | IP protocol version <br>4 or 6 |
| subnets.ipv6_address_mode | Body | String | The address assignment mode of IPv6 |
| subnets.cidr | Body | String | CIDR of subnet |
| subnets.id | Body | UUID | Subnet ID |
| subnets.subnetpool_id | Body | UUID | Subnet Pool ID |
| subnets_links | Body | Array | Information object for pagination <br>Return if `limit` and `offset`are added <br>Includes paths indicating the next list |

<details><summary>Example</summary>
<p>


```json
{
  "subnets": [
    {
      "name": "default",
      "enable_dhcp": true,
      "network_id": "245ff686-4ca2-4176-a069-81013537ac3a",
      "tenant_id": "6cdebe3eb0094910bc41f1d42ebe4cb7",
      "dns_nameservers": [],
      "gateway_ip": "192.168.0.1",
      "ipv6_ra_mode": null,
      "allocation_pools": [
        {
          "start": "192.168.0.2",
          "end": "192.168.0.254"
        }
      ],
      "host_routes": [],
      "ip_version": 4,
      "ipv6_address_mode": null,
      "cidr": "192.168.0.0/24",
      "id": "b83863ff-0355-4c73-8c10-0bdf66a69aab",
      "subnetpool_id": null
    }
  ]
}
```

</p>
</details>

---

## Port
### List Ports 
Return the list of ports. 
```
GET /v2.0/ports
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| id | Query | UUID | - | ID of port IP to query |
| status | Query | Enum | - | Port status to query <br>Either **ACTIVE** or **DOWN**. |
| display_name | Query | UUID | - | Port name to query |
| admin_state | Query | Boolean | - | Administrator control status of port to query |
| network_id | Query | UUID | - | Network ID of port to query |
| tenant_id | Query | String | - | Tenant ID of port to query |
| device_owner | Query | String | - | Resource type for the port to query |
| mac_address | Query | String | - | MAC address of port to query |
| port_id | Query | UUID | - | ID of port to query |
| security_groups | Query | UUID | - | Security group ID of port to query |
| device_id | Query | UUID | - | Resource ID for the port to query |
| fields | Query | String | - | Field name of port to query <br>e.g.) `fields=id&fields=name` |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| ports | Body | Array | List of port information objects |
| ports.status | Body | Enum | Port status<br>Either **ACTIVE**, or **DOWN** |
| ports.name | Body | String | Port name |
| ports.allowed_address_pairs | Body | Array | List of address pairs of port |
| ports.admin_state_up | Body | Boolean | Admin control status of port |
| ports.network_id | Body | UUID | Network ID of port |
| ports.tenant_id | Body | String | Tenant ID |
| ports.extra_dhcp_opts | Body | Array | Additional DHCP setting |
| ports.binding:vnic_type | Body | String | Port type |
| ports.device_owner | Body | String | Resource type using port |
| ports.mac_address | Body | String | MAC address of port |
| ports.port_security_enabled | Body | Boolean | Security status of port <br>If enabled, available to be applied to security group |
| ports.fixed_ips | Body | Array | List of fixed IPs of port |
| ports.fixed_ips.subnet_id | Body | UUID | Subnet ID of fixed IP of port |
| ports.fixed_ips.ip_address | Body | String | Fixed IP address of port |
| ports.id | Body | UUID | Port ID |
| ports.security_groups | Body | Array | List of security group IDs of port |
| ports.device_id | Body | UUID | Resource ID using port |

<details><summary>Example</summary>
<p>


```json
{
  "ports": [
    {
      "status": "ACTIVE",
      "name": "",
      "allowed_address_pairs": [],
      "admin_state_up": true,
      "network_id": "c46ee4e8-c9fa-462e-8a3b-55b1f11dd8f8",
      "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
      "extra_dhcp_opts": [],
      "binding:vnic_type": "normal",
      "device_owner": "compute:kr-pub-a",
      "mac_address": "fa:16:3e:1a:ed:34",
      "port_security_enabled": true,
      "fixed_ips": [
        {
          "subnet_id": "eb166bdf-dd99-4b02-ac8e-64be21dff2d5",
          "ip_address": "192.168.0.15"
        }
      ],
      "id": "2a83bc25-ed76-4a2b-83b7-a4408aa99c4a",
      "security_groups": [
        "877b092c-2a31-4509-8d23-deeb02d95633"
      ],
      "device_id": "10b9643d-9bc8-4b0f-a5dd-cfcb4033d70b"
    }
  ]
}
```

</p>
</details>

---

### Get Port 

```
GET /v2.0/ports/{portId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| portId | URL | UUID | O | Port ID |
| tokenId | Header | String | O | Token ID |
| fields | Query | String | - | Field name of port to query <br>e.g.) `fields=id&fields=name` |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| port | Body | Object | Port information object |
| port.status | Body | Enum | Port status <br>Either **ACTIVE** or **DOWN** |
| port.name | Body | String | Port name |
| port.allowed_address_pairs | Body | Array | List of address pairs of port |
| port.admin_state_up | Body | Boolean | Admin control status of port |
| port.network_id | Body | UUID | Network ID of port |
| port.tenant_id | Body | String | Tenant ID |
| port.extra_dhcp_opts | Body | Array | Additional DHCP setting |
| port.binding:vnic_type | Body | String | Port type |
| port.device_owner | Body | String | Resource type using port |
| port.mac_address | Body | String | MAC address of port |
| port.port_security_enabled | Body | Boolean | Security status of port <br>If enabled, available to be applied to security group |
| port.fixed_ips | Body | Array | List of fixed IPs of port |
| port.fixed_ips.subnet_id | Body | UUID | Subnet ID of fixed IP of port |
| port.fixed_ips.ip_address | Body | String | Fixed IP address of port |
| port.id | Body | UUID | Port ID |
| port.security_groups | Body | Array | List of security group IDs of port |
| port.device_id | Body | UUID | Resource ID using port |

<details><summary>Example</summary>
<p>


```json
{
  "port": {
    "status": "ACTIVE",
    "name": "",
    "allowed_address_pairs": [],
    "admin_state_up": true,
    "network_id": "c46ee4e8-c9fa-462e-8a3b-55b1f11dd8f8",
    "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
    "extra_dhcp_opts": [],
    "binding:vnic_type": "normal",
    "device_owner": "compute:kr-pub-a",
    "mac_address": "fa:16:3e:1a:ed:34",
    "port_security_enabled": true,
    "fixed_ips": [
      {
        "subnet_id": "eb166bdf-dd99-4b02-ac8e-64be21dff2d5",
        "ip_address": "192.168.0.15"
      }
    ],
    "id": "2a83bc25-ed76-4a2b-83b7-a4408aa99c4a",
    "security_groups": [
      "877b092c-2a31-4509-8d23-deeb02d95633"
    ],
    "device_id": "10b9643d-9bc8-4b0f-a5dd-cfcb4033d70b"
  }
}
```

</p>
</details>

---

### Create Port
Create a new port: new port can be used to create an instance. 
```
POST /v2.0/ports
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| port | Body | Object | O | Object requesting of creating a port |
| port.name | Body | String | - | Port name |
| port.network_id | Body | UUID | O | Network ID of port |
| port.admin_state_up | Body | Boolean | - | Admin control status of port |
| port.mac_address | Body | String | - | MAC address of port |
| port.port_id | Body | UUID | - | ID of port attached with floating IP |
| port.fixed_ips | Body | Array | - | List of fixed IPs of port |
| port.fixed_ips.subnet_id | Body | UUID | - | Subnet ID of fixed IP of port |
| port.fixed_ips.ip_address | Body | String | - | Fixed IP address of port |
| port.security_groups | Body | Array | - | List of security group IDs of port |
| port.allowed_address_pairs | Body | Array | - | List of address pairs of port |
| port.allowed_address_pairs.ip_address | Body | String | - | IP address of port |
| port.allowed_address_pairs.mac_address | Body | String | - | MAC address of port |
| port.extra_dhcp_opts | Body | Array | - | Additional DHCP setting |
| port.device_owner | Body | String | - | Resource type using port |
| port.device_id | Body | UUID | - | Resource ID using port |

<details><summary>Example</summary>
<p>


```json
{
    "port": {
        "network_id": "a87cc70a-3e15-4acf-8205-9b711a3531b7",
        "name": "private-port",
        "admin_state_up": true
    }
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| port | Body | Array | Port information object |
| port.status | Body | Enum | Port status <br>Either **ACTIVE** or **DOWN** |
| port.name | Body | String | Port name |
| port.allowed_address_pairs | Body | Array | List of address pairs of port |
| port.admin_state_up | Body | Boolean | Admin control status of port |
| port.network_id | Body | UUID | Network ID of port |
| port.tenant_id | Body | String | Tenant ID |
| port.extra_dhcp_opts | Body | Array | Additional DHCP setting |
| port.binding:vnic_type | Body | String | Port type |
| port.device_owner | Body | String | Resource type using port |
| port.mac_address | Body | String | MAC address of port |
| port.port_security_enabled | Body | Boolean | Security status of port <br>If enabled, available to be applied to security group |
| port.fixed_ips | Body | Array | List of fixed IPs of port |
| port.fixed_ips.subnet_id | Body | UUID | Subnet ID of fixed IP of port |
| port.fixed_ips.ip_address | Body | String | Fixed IP address of port |
| port.id | Body | UUID | Port ID |
| port.security_groups | Body | Array | List of security group IDs of port |
| port.device_id | Body | UUID | Resource ID using port |

<details><summary>Example</summary>
<p>


```json
{
    "port": {
        "status": "DOWN",
        "name": "private-port",
        "allowed_address_pairs": [],
        "admin_state_up": true,
        "network_id": "a87cc70a-3e15-4acf-8205-9b711a3531b7",
        "tenant_id": "d6700c0c9ffa4f1cb322cd4a1f3906fa",
        "device_owner": "",
        "mac_address": "fa:16:3e:c9:cb:f0",
        "fixed_ips": [
            {
                "subnet_id": "a0304c3a-4f08-4c43-88af-d796509c97d2",
                "ip_address": "10.0.0.2"
            }
        ],
        "id": "65c0ee9f-d634-4522-8954-51021b570b0d",
        "security_groups": [
            "f0ac4394-7e4a-4409-9701-ba8be283dbc3"
        ],
        "device_id": ""
    }
}
```

</p>
</details>

---

### Delete Port 
Delete a specified port. 
```
DELETE /v2.0/ports/{portId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| portId | URL | UUID | O | Port ID |
| tokenId | Header | String | O | Token ID |

#### Response
This API does not return a response body. 

---

## Floating IP
### List Floating IPs 
Return the list of floating IPs. 
```
GET /v2.0/floatingips
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| id | Query | UUID | - | ID of floating IP to query |
| status | Query | Enum | - | Status of floating IP to query <br>**ACTIVE**: Attached to instance <br>**DOWN**: Not attached to instance <br>**ERROR**: Attached to instance or failed in assignment |
| tenant_id | Query | String | - | Tenant ID of floating IP to query |
| floating_network_id  | Query | UUID | - | ID of external network including floating IP to query |
| fixed_ip_address | Query | String | - | Fixed IP address associated with floating IP to query |
| floating_ip_address | Query | String | - | Address of floating IP to query |
| port_id | Query | UUID | - | ID of port attached with floating IP to query |
| sort_dir | Query | Enum | - | Sorting direction of floating IP to query <br>Sort by the field as specified by `sort_key`<br>Either **asc**, or **desc** |
| sort_key | Query | String | - | Sorting key of floating IP to query<br>Sort in the direction as specified by `sort_dir` |
| fields | Query | String | - | Field name of floating IP to query <br>e.g.) `fields=id&fields=name` |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| floatingips | Body | Array | List of information objects of floating IP |
| floatingips.floating_network_id | Body | UUID | ID of external network including floating IP |
| floatingips.router_id | Body | UUID | ID of router attached with floating IP |
| floatingips.fixed_ip_address | Body | String | Fixed IP address associated with floating IP |
| floatingips.floating_ip_address | Body | String | Floating IP address |
| floatingips.tenant_id | Body | String | Tenant ID |
| floatingips.status | Body | Enum | Status of floating IP <br>**ACTIVE**: Attached to instance <br>**DOWN**: Not attached to instance <br>**ERROR**: Attached to instance or failed in assignment |
| floatingips.port_id | Body | UUID | ID of port attached with floating IP |
| floatingips.id | Body | UUID | ID of floating IP |

<details><summary>Example</summary>
<p>


```json
{
  "floatingips": [
    {
      "floating_network_id": "4b61db01-8183-4540-b2a3-47254a58298d",
      "router_id": null,
      "fixed_ip_address": null,
      "floating_ip_address": "133.186.242.214",
      "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
      "status": "DOWN",
      "port_id": null,
      "id": "fed3fcf6-59b1-4f43-93e5-23a47cb5452e"
    }
  ]
}
```

</p>
</details>

---

### Get Floating IP
Return information for specified floating IP. 
```
GET /v2.0/floatingips/{floatingIpId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| floatingIpId | URL | UUID | O | ID of floating IP |
| tokenId | Header | String | O | Token ID |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| floatingip | Body | Object | Information object of floating IP |
| floatingip.floating_network_id | Body | UUID | ID of external network including floating IP |
| floatingip.router_id | Body | UUID | ID of router attached with floating IP |
| floatingip.fixed_ip_address | Body | String | Fixed IP address associated with floating IP |
| floatingip.floating_ip_address | Body | String | Floating iP address |
| floatingip.tenant_id | Body | String | Tenant ID |
| floatingip.status | Body | Enum | Status of floating IP<br>**ACTIVE**: Attached to instance <br>**DOWN**: Not attached to instance<br>**ERROR**: Attached to instance or failed in assignment |
| floatingip.port_id | Body | UUID | ID of port attached with floating IP |
| floatingip.id | Body | UUID | ID of floating IP |

<details><summary>Example</summary>
<p>


```json
{
  "floatingip": {
    "floating_network_id": "4b61db01-8183-4540-b2a3-47254a58298d",
    "router_id": null,
    "fixed_ip_address": null,
    "floating_ip_address": "133.186.242.214",
    "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
    "status": "DOWN",
    "port_id": null,
    "id": "fed3fcf6-59b1-4f43-93e5-23a47cb5452e"
  }
}
```

</p>
</details>

---

### Create Floating IP
Create a floating IP. 
```
POST /v2.0/floatingips
X-Auth-Token: {tokenId}
```

#### Request	

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| floatingip | Body | Object | O | Object requesting of creating floating IP |
| floatingip.floating_network_id | Body | UUID | O | ID of external network including floating IP |
| floatingip.port_id | Body | UUID | - | ID of port to be attached with floating IP |

<details><summary>Example</summary>
<p>


```json
{
  "floatingip": {
    "floating_network_id": "4b61db01-8183-4540-b2a3-47254a58298d",
    "port_id": null
  }
}
```

</p>
</details>

#### Response		

| Name | Type | Format | Description |
|---|---|---|---|
| floatingip | Body | Object | Information object of floating IP |
| floatingip.floating_network_id | Body | UUID | ID of external network including floating IP |
| floatingip.router_id | Body | UUID | ID of router attached to floating IP |
| floatingip.fixed_ip_address | Body | String | Fixed IP address associated with floating IP |
| floatingip.floating_ip_address | Body | String | Floating IP address |
| floatingip.tenant_id | Body | String | Tenant ID |
| floatingip.status | Body | Enum | Status of floating IP <br>**ACTIVE**: Attached to instance <br>**DOWN**: Not attached to instance <br>**ERROR**: Attached to instance or failed in assignment |
| floatingip.port_id | Body | UUID | Port ID associated with floating IP |
| floatingip.id | Body | UUID | ID of floating IP |

<details><summary>Example</summary>
<p>


```json
{
  "floatingip": {
    "floating_network_id": "4b61db01-8183-4540-b2a3-47254a58298d",
    "router_id": null,
    "fixed_ip_address": null,
    "floating_ip_address": "133.186.242.214",
    "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
    "status": "DOWN",
    "port_id": null,
    "id": "fed3fcf6-59b1-4f43-93e5-23a47cb5452e"
  }
}
```

</p>
</details>

---

### Attach/Detach Floating IP
```
PUT /v2.0/floatingips/{floatingIpId}
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| floatingIpId | URL | UUID | ID of floating IP ||
| tokenId | Header | String | O | Token ID |
| floatingip | Body | Object | O | Object requesting of modifying floating IP |
| floatingip.port_id | Body | UUID | O | ID of port to be attached to floating IP<br>Enter `null` to detach |
| floatingip.fixed_ip_address | Body | String | - | Fixed IP address<br>To specify a particular IP, if many IPs are assigned to a port to be attached or detached |

<details><summary>Example</summary>
<p>


```json
{
    "floatingip": {
        "port_id": "af41e9f7-18ae-43c5-8b7e-7026f792bf3a"
    }
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| floatingip | Body | Object | Information object of floating IP |
| floatingip.floating_network_id | Body | UUID | ID of external network including floating IP |
| floatingip.router_id | Body | UUID | ID of router attached to floating IP |
| floatingip.fixed_ip_address | Body | String | Fixed IP address associated with floaitng IP |
| floatingip.floating_ip_address | Body | String | Floating IP address |
| floatingip.tenant_id | Body | String | Tenant ID |
| floatingip.status | Body | Enum | Status of floating IP |
| floatingip.port_id | Body | UUID | ID of port attached to floating IP |
| floatingip.id | Body | UUID | ID of floating IP |

<details><summary>Example</summary>
<p>


```json
{
  "floatingip": {
    "floating_network_id": "b04b1c31-f2e9-4ae0-a264-02b7d61ad618",
    "router_id": "4337119f-8c72-40bf-818a-21258ecb86db",
    "fixed_ip_address": "192.168.22.96",
    "floating_ip_address": "133.186.147.40",
    "tenant_id": "f5073eaa26b64cffbee89411df94ce01",
    "status": "DOWN",
    "port_id": "af41e9f7-18ae-43c5-8b7e-7026f792bf3a",
    "id": "5338b5b2-9d80-46b5-ba13-2fd13f5c498a"
  }
}
```

</p>
</details>

---

### Delete Floating IP
Delete a specified floating IP. 
```
DELETE /v2.0/floatingips/{floatingIpId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| floatingIpId | URL | UUID | O | ID of floating IP |
| tokenId | Header | String | O | Token ID |

#### Response
This API does not return a response body. 

---

## Security Group
### List Security Groups 
```
GET /v2.0/security-groups
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| id | Query | UUID | - | Security group ID to query |
| tenant_id | Query | String | - | Tenant ID of security group to query |
| name | Query | String | - | Name of security group to query |
| sort_dir | Query | Enum | - | Sorting direction of security group to query <br>Sort by the field specified by`sort_key`<br>Either **asc** or **desc** |
| sort_key | Query | String | - | Sorting key of security group to query <br>Sort in the direction as specified by`sort_dir` |
| fields | Query | String | - | Field name of security group to query <br>e.g.) `fields=id&fields=name` |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| security_groups | Body | Array | Object of security group list |
| security_groups.tenant_id | Body | String | Tenant ID |
| security_groups.description | Body | String | Description of security group |
| security_groups.id | Body | UUID | Security group ID |
| security_groups.security_group_rules | Body | Array | List of security group rules |
| security_groups.name | Body | String | Security group name |

<details><summary>Example</summary>
<p>


```json
{
  "security_groups": [
    {
      "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
      "description": "Default security group",
      "id": "877b092c-2a31-4509-8d23-deeb02d95633",
      "security_group_rules": [
        {
          "direction": "ingress",
          "protocol": null,
          "description": "",
          "port_range_max": null,
          "id": "0c7279cd-657e-43cd-a635-6886ca3a8acd",
          "remote_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
          "remote_ip_prefix": null,
          "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
          "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
          "port_range_min": null,
          "ethertype": "IPv4"
        },
        {
          "direction": "egress",
          "protocol": null,
          "description": "",
          "port_range_max": null,
          "id": "4c5ae06e-44f0-4ea9-a999-29473873bca2",
          "remote_group_id": null,
          "remote_ip_prefix": null,
          "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
          "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
          "port_range_min": null,
          "ethertype": "IPv6"
        },
        {
          "direction": "egress",
          "protocol": null,
          "description": "",
          "port_range_max": null,
          "id": "4e21389a-bb3c-469c-8139-29da582bc3c5",
          "remote_group_id": null,
          "remote_ip_prefix": null,
          "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
          "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
          "port_range_min": null,
          "ethertype": "IPv4"
        },
        {
          "direction": "ingress",
          "protocol": null,
          "description": "",
          "port_range_max": null,
          "id": "72af180f-c425-4ec4-b7a3-90f86d4ce145",
          "remote_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
          "remote_ip_prefix": null,
          "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
          "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
          "port_range_min": null,
          "ethertype": "IPv6"
        }
      ],
      "name": "default"
    }
  ]
}
```

</p>
</details>

---

### Get Security Group
```
GET /v2.0/security-groups/{securityGroupId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| securityGroupId | Query | UUID | O | ID of security group to query |
| tokenId | Header | String | O | Token ID |
| fields | Query | String | - | Field name of security group to query <br>Return specified fields only to response <br>e.g.) `fields=id&fields=name` |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| security_group | Body | Object | Security group object |
| security_group.tenant_id | Body | String | Tenant ID |
| security_group.description | Body | String | Security group description |
| security_group.id | Body | UUID | Security group ID |
| security_group.security_group_rules | Body | Array | List of security group rules |
| security_group.name | Body | String | Security group name |

<details><summary>Example</summary>
<p>


```json
{
  "security_group": {
    "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
    "description": "Default security group",
    "id": "877b092c-2a31-4509-8d23-deeb02d95633",
    "security_group_rules": [
      {
        "direction": "ingress",
        "protocol": null,
        "description": "",
        "port_range_max": null,
        "id": "0c7279cd-657e-43cd-a635-6886ca3a8acd",
        "remote_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
        "remote_ip_prefix": null,
        "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
        "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
        "port_range_min": null,
        "ethertype": "IPv4"
      },
      {
        "direction": "egress",
        "protocol": null,
        "description": "",
        "port_range_max": null,
        "id": "4c5ae06e-44f0-4ea9-a999-29473873bca2",
        "remote_group_id": null,
        "remote_ip_prefix": null,
        "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
        "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
        "port_range_min": null,
        "ethertype": "IPv6"
      },
      {
        "direction": "egress",
        "protocol": null,
        "description": "",
        "port_range_max": null,
        "id": "4e21389a-bb3c-469c-8139-29da582bc3c5",
        "remote_group_id": null,
        "remote_ip_prefix": null,
        "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
        "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
        "port_range_min": null,
        "ethertype": "IPv4"
      }
      {
        "direction": "ingress",
        "protocol": null,
        "description": "",
        "port_range_max": null,
        "id": "72af180f-c425-4ec4-b7a3-90f86d4ce145",
        "remote_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
        "remote_ip_prefix": null,
        "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
        "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
        "port_range_min": null,
        "ethertype": "IPv6"
      }
    ],
    "name": "default"
  }
}
```

</p>
</details>

---

### Create Security Group

Create a new security group: new security group, by default, includes the egress security group rules. 

```
POST /v2.0/security-groups
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| security_group | Body | Object | O | Object requesting of creating security group |
| description | Body | String | - | Security group description |
| name | Body | String | - | Security group name |

<details><summary>Example</summary>
<p>


```json
{
    "security_group": {
        "name": "webservers",
        "description": "security group for webservers"
    }
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| security_group | Body | Object | Security group object |
| security_group.tenant_id | Body | String | Tenant ID |
| security_group.description | Body | String | Security group description |
| security_group.id | Body | UUID | Security group ID |
| security_group.security_group_rules | Body | Array | List of security group rules |
| security_group.name | Body | String | Security group name |

<details><summary>Example</summary>
<p>


```json
{
  "security_group": {
    "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
    "description": "security group for webservers",
    "id": "148cfc28-a58c-497f-aaab-c610bf7b5f18",
    "security_group_rules": [
      {
        "direction": "egress",
        "protocol": null,
        "description": null,
        "port_range_max": null,
        "id": "057e031a-ec2a-4bee-859a-6bc1d88c57d0",
        "remote_group_id": null,
        "remote_ip_prefix": null,
        "security_group_id": "148cfc28-a58c-497f-aaab-c610bf7b5f18",
        "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
        "port_range_min": null,
        "ethertype": "IPv4"
      },
      {
        "direction": "egress",
        "protocol": null,
        "description": null,
        "port_range_max": null,
        "id": "cd37d4a7-75ac-4246-95cf-e93b37b75a9f",
        "remote_group_id": null,
        "remote_ip_prefix": null,
        "security_group_id": "148cfc28-a58c-497f-aaab-c610bf7b5f18",
        "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
        "port_range_min": null,
        "ethertype": "IPv6"
      }
    ],
    "name": "webservers"
  }
}
```

</p>
</details>

---

### Modify Security Group
Modify an existing security group. 
```
PUT /v2.0/security-groups/{securityGroupId}
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| securityGroupId | URL | UUID | O | Security group ID |
| security_group | Body | Object | O | Object requesting of modifying security group |
| description | Body | String | - | Security group description |
| name | Body | String | - | Security group name |

<details><summary>Example</summary>
<p>


```json
{
    "security_group": {
        "name": "new-webservers",
        "description": "security group for new webservers"
    }
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| security_group | Body | Object | Security group object |
| security_group.tenant_id | Body | String | Tenant ID |
| security_group.description | Body | String | Security group description |
| security_group.id | Body | UUID | Security group ID |
| security_group.security_group_rules | Body | Array | List of security group rules |
| security_group.name | Body | String | Security group name |

<details><summary>Example</summary>
<p>


```json
{
  "security_group": {
    "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
    "description": "security group for new webservers",
    "id": "148cfc28-a58c-497f-aaab-c610bf7b5f18",
    "security_group_rules": [
      {
        "direction": "egress",
        "protocol": null,
        "description": null,
        "port_range_max": null,
        "id": "057e031a-ec2a-4bee-859a-6bc1d88c57d0",
        "remote_group_id": null,
        "remote_ip_prefix": null,
        "security_group_id": "148cfc28-a58c-497f-aaab-c610bf7b5f18",
        "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
        "port_range_min": null,
        "ethertype": "IPv4"
      },
      {
        "direction": "egress",
        "protocol": null,
        "description": null,
        "port_range_max": null,
        "id": "cd37d4a7-75ac-4246-95cf-e93b37b75a9f",
        "remote_group_id": null,
        "remote_ip_prefix": null,
        "security_group_id": "148cfc28-a58c-497f-aaab-c610bf7b5f18",
        "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
        "port_range_min": null,
        "ethertype": "IPv6"
      }
    ],
    "name": "new-webservers"
  }
}
```

</p>
</details>

---

### Delete Security Group
Delete specified security group. 
```
DELETE /v2.0/security-groups/{securityGroupId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| securityGroupId | URL | UUID | O | Security group ID |
| tokenId | Header | String | O | Token ID |

#### Response
This API does not return a response body. 

---

## Security Rule
### List Security Rules 
```
GET /v2.0/security-group-rules
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| id | Query | UUID | - | Security rule ID to query |
| remote_group_id | Query | UUID | - | Remote security group ID of security rule to query |
| protocol | Query | String | - | Protocol of security rule to query |
| direction | Query | Enum | - | Packet direction to which security rule to query is applied<br>**ingress** or **egress** |
| ethertype | Query | Enum | - | The `Ethertype` value of network traffic of security rule to query<br>**IPv4** or **IPv6** |
| port_range_max | Query | Integer | - | Maximum value within port range of security rule to query |
| port_range_min | Query | Integer | - | Minimum value within port range of security rule to query |
| security_group_id | Query | UUID | - | Security group ID including security rule to query |
| tenant_id | Query | String | - | Tenant ID of security rule to query |
| remote_ip_prefix | Query | String | - | Prefix of destination IP of security rule to query |
| description | Query | String | - | Description of security rule to query |
| sort_dir | Query | Enum | - | Sorting direction of security rule to query <br>Sort by the field specified by `sort_key`<br>Either **asc** or **desc** |
| sort_key | Query | String | - | Sorting key of security rule to query <br>Sort in the direction specified by `sort_dir` |
| fields | Query | String | - | Field name of security rule to query<br>e.g.) `fields=id&fields=name` |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| security_group_rules | Body | Array | List of security rule objects |
| security_group_rules.direction | Body | Enum | Packet direction to which security rule is applied <br>**ingress** or **egress** |
| security_group_rules.ethertype | Body | Enum | The `Ethertype` value of network traffic of security rule <br>**IPv4** or **IPv6** |
| security_group_rules.protocol | Body | String | Protocol name of security rule |
| security_group_rules.description | Body | String | Security rule description |
| security_group_rules.port_range_max | Body | Integer | Maximum value within port range of security rule |
| security_group_rules.port_range_min | Body | Integer | Minimum value within port range of security rule |
| security_group_rules.remote_group_id | Body | UUID | Remote security group ID of security rule |
| security_group_rules.remote_ip_prefix | Body | Enum | Prefix of destination IP of security rule |
| security_group_rules.security_group_id | Body | UUID | Security group ID including security rule |
| security_group_rules.tenant_id | Body | String | Tenant ID |
| security_group_rules.id | Body | UUID | Security rule ID |

<details><summary>Example</summary>
<p>


```json
{
  "security_group_rules": [
    {
      "direction": "ingress",
      "protocol": "tcp",
      "description": "",
      "port_range_max": 65535,
      "id": "8eb7775f-1193-472a-98bd-e0599f94a64d",
      "remote_group_id": null,
      "remote_ip_prefix": "0.0.0.0/0",
      "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
      "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
      "port_range_min": 1,
      "ethertype": "IPv4"
    }
  ]
}
```

</p>
</details>

---

### Get Security Rule
```
GET /v2.0/security-group-rules/{securityGroupRuleId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body. 

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| securityGroupRuleId | URL | UUID | O | Security rule ID |
| tokenId | Header | String | O | Token ID |
| fields | Query | String | - | Field name of security rule to query <br>e.g.) `fields=id&fields=name` |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| security_group_rule | Body | Object | Security rule object |
| security_group_rule.direction | Body | Enum | Packet direction to which security rule is applied<br>**ingress** or **egress** |
| security_group_rule.ethertype | Body | Enum | The  `Ethertype` value of network traffic of security rule <br>**IPv4** or **IPv6** |
| security_group_rule.protocol | Body | String | Protocol name of security rule |
| security_group_rule.description | Body | String | Security rule description |
| security_group_rule.port_range_max | Body | Integer | Maximum value within port range of security rule to query |
| security_group_rule.port_range_min | Body | Integer | Minimum value within port range of security rule to query |
| security_group_rule.remote_group_id | Body | UUID | Remote security group ID of security rule |
| security_group_rule.remote_ip_prefix | Body | Enum | Prefix of destination IP of security rule |
| security_group_rule.security_group_id | Body | UUID | Security group ID including security rule |
| security_group_rule.tenant_id | Body | String | Tenant ID |
| security_group_rule.id | Body | UUID | Security rule ID |

<details><summary>Example</summary>
<p>


```json
{
  "security_group_rule": {
    "direction": "ingress",
    "protocol": "tcp",
    "description": "",
    "port_range_max": 65535,
    "id": "8eb7775f-1193-472a-98bd-e0599f94a64d",
    "remote_group_id": null,
    "remote_ip_prefix": "0.0.0.0/0",
    "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
    "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
    "port_range_min": 1,
    "ethertype": "IPv4"
  }
}
```

</p>
</details>

---

### Create Security Rule

Create a new security group rule. It is available to create security rules only for IPv4. 

```
POST /v2.0/security-group-rules
X-Auth-Token: {tokenId}
```

#### Request	

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| security_group_rule | Body | Object | O | Object requesting of creating security rules |
| security_group_rule.remote_group_id | Body | UUID | - | Remote security group ID of security rule |
| security_group_rule.direction | Body | Enum | O | Packet direction to which security rule is applied<br>**ingress**, **egress** |
| security_group_rule.ethertype | Body | Enum | - | If left blank, specified with`IPv4`. |
| security_group_rule.protocol | Body | String | - | Protocol name of security rule: to be applied to all protocols, if it is omitted. |
| security_group_rule.port_range_max | Body | Integer | - | Maximum value within port range of security rule |
| security_group_rule.port_range_min | Body | Integer | - | Minimum value within port range of security rule |
| security_group_rule.security_group_id | Body | UUID | O | ID of security group including security rules |
| security_group_rule.remote_ip_prefix | Body | Enum | - | Prefix of destination IP of security rule |
| security_group_rule.description | Body | String | - | Security rule description |

<details><summary>Example</summary>
<p>


```json
{
    "security_group_rule": {
        "direction": "ingress",
        "port_range_min": "80",
        "ethertype": "IPv4",
        "port_range_max": "80",
        "protocol": "tcp",
        "remote_group_id": "85cc3048-abc3-43cc-89b3-377341426ac5",
        "security_group_id": "a7734e61-b545-452d-a3cd-0189cbd9747a"
    }
}
```

</p>
</details>

#### Response	

| Name | Type | Format | Description |
|---|---|---|---|
| security_group_rule | Body | Object | Security rule object |
| security_group_rule.direction | Body | Enum | Packet direction to which security rules are applied <br>**ingress** or **egress** |
| security_group_rule.ethertype | Body | Enum | The `Ethertype` of network traffic of security rules <br>**IPv4** or **IPv6** |
| security_group_rule.protocol | Body | String | Protocol name of security rule |
| security_group_rule.description | Body | String | Security rule description |
| security_group_rule.port_range_max | Body | Integer | Maximum value within port range of security rule to query |
| security_group_rule.port_range_min | Body | Integer | Minimum value within port range of security rule to query |
| security_group_rule.remote_group_id | Body | UUID | ID of remote security group of security rule |
| security_group_rule.remote_ip_prefix | Body | Enum | Prefix of destination IP of security rule |
| security_group_rule.security_group_id | Body | UUID | ID of security group including security rule |
| security_group_rule.tenant_id | Body | String | Tenant ID |
| security_group_rule.id | Body | UUID | Security rule ID |

<details><summary>Example</summary>
<p>


```json
{
  "security_group_rule": {
    "direction": "ingress",
    "protocol": "tcp",
    "description": "",
    "port_range_max": 65535,
    "id": "8eb7775f-1193-472a-98bd-e0599f94a64d",
    "remote_group_id": null,
    "remote_ip_prefix": "0.0.0.0/0",
    "security_group_id": "877b092c-2a31-4509-8d23-deeb02d95633",
    "tenant_id": "19eeb40d58684543aef29cbb5ebfe8f0",
    "port_range_min": 1,
    "ethertype": "IPv4"
  }
}
```

</p>
</details>

---

### Delete Security Rule
Delete a specified security rule.
```
DELETE /v2.0/security-group-rules/{securityGroupRuleId}
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| securityGroupRuleId | URL | UUID | O | ID of security rules |
| tokenId | Header | String | O | Token ID |

#### Response
This API does not return a request body. 
