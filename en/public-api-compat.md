## Network > VPC > Openstack Compatible API Guide

NHN Cloud Network services provide APIs compatible with OpenStack neutron APIs.
The OpenStack-compatible APIs are provided as follows.

| OpenStack API Methods | Usage |
| --- | --- |
| GET networks | List networks |
| GET subnets | List subnets |
| GET ports | List ports |
| POST port | Create a port |
| PUT port | Change a port |
| DELETE port | Delete a port |

The fields included in requests and responses from OpenStack-compatible APIs are limited to the fields specified in this document among those provided by the OpenStack neutron APIs in accordance with NHN Cloud policies.

To use the API, you need the API endpoint and token. Prepare information required to use the API by referring to [API Preparations](/Compute/Compute/en/identity-api/)

For Openstack compatible APIs, the `network` type endpoint is used. For more details, see `serviceCatalog` from the response of token issuance.

| Type | Region | Endpoint |
|---|---|---|
| network | Korea (Pangyo) Region<br>Korea (Pyeongchon) Region<br>Japan (Tokyo) Region<br>USA (California) Region | https://kr1-api-network-infrastructure.nhncloudservice.com<br>https://kr2-api-network-infrastructure.nhncloudservice.com<br>https://jp1-api-network-infrastructure.nhncloudservice.com<br>https://us1-api-network-infrastructure.nhncloudservice.com |


## Network
### List Networks
Returns the list of available networks.
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
| provider:network_type | Query | Enum | - | Network type to query<br>Either `flat` or `vlan` |
| router:external | Query | Boolean | - | Whether network is externally connected |
| shared | Query | Boolean | - | Whether to share network to query |
| tenant_id | Query | String | - | Tenant ID to which network to query is included |
| sort_dir | Query | Enum | - | Sorting direction of network to query<br>Sort by the field specified by `sort_key`<br>Either **asc**, or **desc** |
| sort_key | Query | String | - | Sorting key of network to query<br>Sort in the direction as specified by `sort_dir` |
| fields | Query | String | - | Field name of network to query<br>e.g.) `fields=id&fields=name` |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| networks | Body | Array | List of network information objects |
| networks.status | Body | Enum | Network status<br>One of **ACTIVE**, **DOWN**, **BUILD**, and **ERROR** |
| networks.subnets | Body | Array | List of subnet IDs included to network |
| networks.name | Body | String | Network name |
| networks.router:external | Body | Boolean | Whether network is externally connected |
| networks.tenant_id | Body | String | Tenant ID |
| networks.admin_state_up | Body | Boolean | Status of admin control<br>`true`: Available<br>`false`: Unavailable |
| networks.mtu | Body | Integer | Maximum Transmission Unit |
| networks.shared | Body | Boolean | Whether to share network |
| networks.port_security_enabled | Body | Boolean | Whether network port is secured<br>Decide whether to enable security of port created in the network |
| networks.id | Body | String | Network ID |
| networks.name | Body | String | Network name |
| networks_links | Body | Array | Information object for pagination<br>Return when `limit` or `offset` is added<br>Includes paths indicating the next list |

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
      "subnets": [ "6b3f7d6d-df61-4345-beb5-1621fd274659",
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
Returns the list of available subnets.
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
| sort_dir | Query | Enum | - | Sorting direction of subnet to query<br>Sort by the field specified by `sort_key`<br>Either **asc**, or **desc** |
| sort_key | Query | String | - | Sorting key of subnet to query<br>Sort in the direction as specified by `sort_dir` |
| fields | Query | String | - | Field name of subnet to query<br>e.g.) `fields=id&fields=name` |

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
| subnets.ip_version | Body | Integer | IP protocol version<br>4 or 6 |
| subnets.ipv6_address_mode | Body | String | The address assignment mode of IPv6 |
| subnets.cidr | Body | String | CIDR of subnet |
| subnets.id | Body | UUID | Subnet ID |
| subnets.subnetpool_id | Body | UUID | Subnet Pool ID |
| subnets_links | Body | Array | Information object for pagination<br>Return when `limit` or `offset` is added<br>Includes paths indicating the next list |

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
| status | Query | Enum | - | Port status to query<br>Either `ACTIVE`, `BUILD` or `DOWN`. |
| name | Query | String | - | Port name to query |
| admin_state | Query | Boolean | - | Administrator control status of port to query |
| network_id | Query | UUID | - | Network ID of port to query |
| tenant_id | Query | String | - | Tenant ID of port to query |
| fixed_ips | Query | array | - | Fixed IP information of the port to query<br>Filterable by IP address (`ip_address`), IP address partial (`ip_address_substr`), or subnet ID (`subnet_id`) |
| mac_address | Query | String | - | MAC address of port to query |
| device_owner | Query | String | - | Type of resource that uses the port to query |
| device_id | Query | UUID | - | Resource ID for the port to query |
| fields | Query | String | - | Field name of port to query<br>e.g.) `fields=id&fields=name` |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
| ports | Body | Array | List of port information objects |
| ports.id | Body | UUID | Port ID |
| ports.name | Body | String | Port name |
| ports.status | Body | Enum | Port status<br>One of `ACTIVE`, `BUILD`, or `DOWN`|
| ports.admin_state_up | Body | Boolean | Admin control status of port |
| ports.network_id | Body | UUID | Network ID of port |
| ports.tenant_id | Body | String | Tenant ID |
| ports.extra_dhcp_opts | Body | Array | Additional DHCP option |
| ports.binding:vnic_type | Body | String | vNIC type connected to the port |
| ports.device_owner | Body | String | Resource type using port |
| ports.device_id | Body | UUID | Resource ID of the resource using the port|
| ports.mac_address | Body | String | MAC address of port |
| ports.fixed_ips.subnet_id | Body | UUID | Subnet ID to which the fixed IP belongs |
| ports.fixed_ips.ip_address | Body | String | Fixed IP address |
| ports.port_security_enabled | Body | Boolean | Security status of the port<br>If enabled, you can set security group, allowed address pairs |
| ports.security_groups | Body | Array | List of security group IDs set on the port |
| ports.allowed_address_pairs | Body | Array | List of allowed address pairs set on the port |

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
      "security_groups": [ "877b092c-2a31-4509-8d23-deeb02d95633"
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
| fields | Query | String | - | Field name of port to query<br>e.g.) `fields=id&fields=name` |

#### Response

| Name | Type | Format | Description |
|---|---|---|---|
|  port | Body | Array | Port information object |
| port.id | Body | UUID | Port ID |
| port.name | Body | String | Port name |
| port.status | Body | Enum | Port status<br>One of `ACTIVE`, `BUILD`, or `DOWN`. |
| port.admin_state_up | Body | Boolean | Admin control status of port |
| port.network_id | Body | UUID | Network ID of port |
| port.tenant_id | Body | String | Tenant ID |
| port.extra_dhcp_opts | Body | Array | Additional DHCP option |
| port.binding:vnic_type | Body | String | vNIC type connected to the port |
| port.device_owner | Body | String | Resource type using port |
| port.device_id | Body | UUID | Resource ID using the port |
| port.mac_address | Body | String | MAC address of port |
| port.fixed_ips.subnet_id | Body | UUID | Subnet ID to which the fixed IP belongs |
| port.fixed_ips.ip_address | Body | String | Fixed IP address |
| port.port_security_enabled | Body | Boolean | Port security status<br>If enabled, you can set security groups and allowed address pairs |
| port.security_groups | Body | Array | List of security group IDs set on the port |
| port.allowed_address_pairs | Body | Array | List of allowed address pairs set on the port |

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
    "security_groups": [ "877b092c-2a31-4509-8d23-deeb02d95633"
    ],
    "device_id": "10b9643d-9bc8-4b0f-a5dd-cfcb4033d70b"
  }
}
      
```

</p>
</details>

---

### Create Port
Creates a new port. The created port can be used when creating an instance.
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
| port.admin_state_up | Body | Boolean | - | Admin control status of port. Default `true` |
| port.fixed_ips.subnet_id | Body | UUID | - | Subnet ID to assign a fixed IP to |
| port.fixed_ips.ip_address | Body | String | - | Fixed IP address|
| port.port_security_enabled | Body | Boolean | - | Whether to enable port security. Default `true` | 
| port.security_groups | Body | Array | - | A list of security group IDs to set on the port. Default `default security group's ID`<br>Settable when port security is enabled |
| port.allowed_address_pairs | Body | Array | - | List of allowed address pairs for the port<br>Settable when port security is enabled |
| port.allowed_address_pairs.ip_address | Body | String | - | IP address to allow |
| port.allowed_address_pairs.mac_address | Body | String | - | MAC address to allow |
| port.extra_dhcp_opts | Body | Array | - | Additional DHCP option |
| port.device_owner | Body | String | - | Resource type using port |
| port.device_id | Body | UUID | - | Resource ID using port. Specify as `network:virtual_ip` when using as a virtual IP|

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
| port.id | Body | UUID | Port ID |
| port.name | Body | String | Port name |
| port.status | Body | Enum | Port status<br>One of `ACTIVE`, `BUILD`, or `DOWN`.. |
| port.admin_state_up | Body | Boolean | Admin control status of port |
| port.network_id | Body | UUID | Network ID of port |
| port.tenant_id | Body | String | Tenant ID |
| port.extra_dhcp_opts | Body | Array | Additional DHCP option |
| port.binding:vnic_type | Body | String | vNIC type connected to the port |
| port.device_owner | Body | String | Resource type using port |
| port.device_id | Body | UUID | Resource ID using the port|
| port.mac_address | Body | String | MAC address of port |
| port.fixed_ips | Body | Array | List of fixed IPs of port |
| port.fixed_ips.subnet_id | Body | UUID | Subnet ID to which the fixed IP belongs |
| port.fixed_ips.ip_address | Body | String | Fixed IP address |
| port.port_security_enabled | Body | Boolean | Security status of the port<br>If enabled, you can set security group, allowed address pairs |
| port.security_groups | Body | Array | List of security group IDs set on the port |
| port.allowed_address_pairs | Body | Array | List of allowed address pairs set on the port |


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

### Change Port
Changes the properties of the specified port.
```
PUT /v2.0/ports/{portId}
X-Auth-Token: {tokenId}
```

#### Request
| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| port | Body | Object | O | Object requesting of creating a port |
| port.name | Body | String | - | Port name |
| port.admin_state_up | Body | Boolean | - | Admin control status of port |
| port.fixed_ips | Body | Array | - | List of fixed IPs of port |
| port.fixed_ips.subnet_id | Body | UUID | - | Subnet ID to which a static IP is assigned |
| port.fixed_ips.ip_address | Body | String | - | Fixed IP address |
| port.port_security_enabled | Body | Boolean | - | Whether to enable port security.<br>Can be changed to `false` after both security group and allow address pairs are removed | 
| port.security_groups | Body | Array | - | List of security group IDs to set on the port. If you enter an empty list, remove all<br>Can be set when port security is enabled |
| port.allowed_address_pairs | Body | Array | - | List of allowed address pairs for the port. Remove all when entering an empty list<br>Can be set when port security is enabled |
| port.allowed_address_pairs.ip_address | Body | String | - | IP addresses to allow |
| port.allowed_address_pairs.mac_address | Body | String | - | MAC addresses to allow |
| port.extra_dhcp_opts | Body | Array | - | Additional DHCP option |

<details><summary>Example</summary>
<p>

```json
{
    "port": {
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
| port.id | Body | UUID | Port ID |
| port.name | Body | String | Port name |
| port.status | Body | Enum | Port status<br>One of `ACTIVE`, `BUILD`, or `DOWN`. |
| port.admin_state_up | Body | Boolean | Admin control status of port |
| port.network_id | Body | UUID | Network ID of port |
| port.tenant_id | Body | String | Tenant ID |
| port.extra_dhcp_opts | Body | Array | Additional DHCP option |
| port.binding:vnic_type | Body | String | Type of vNIC connected to the port |
| port.device_owner | Body | String | Resource type using port |
| port.device_id | Body | UUID | Resource ID using port |
| port.mac_address | Body | String | MAC address of port |
| port.fixed_ips | Body | Array | List of fixed IPs of port |
| port.fixed_ips.subnet_id | Body | UUID | Subnet ID to which the static IP belongs |
| port.fixed_ips.ip_address | Body | String | Fixed IP address |
| port.port_security_enabled | Body | Boolean | Security status of port<br>If enabled, you can set security groups and allowable address pairs |
| port.security_groups | Body | Array | List of security group IDs set on the port |
| port.allowed_address_pairs | Body | Array | List of allowed address pairs set on the port |


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
        "security_groups": [ [
            "f0ac4394-7e4a-4409-9701-ba8be283dbc3"
        ],
        "device_id": ""
    }
}
```

</p>
</details>

### Delete a port

```

```

#### Request


| Name | Type | Format | Required | Description |
|---|---|---|---|---|
|  |  | UUID | O |  |
| tokenId | Header | String | O | Token ID |

### Delete Port
Deletes a specified port.
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

