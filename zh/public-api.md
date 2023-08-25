## Network > VPC > API v2 Guide

To use the API, you need the API endpoint and token. Prepare information required to use the API by referring to [API Preparations](/Compute/Compute/en/identity-api/)

For VPC API, the `network` type endpoint is used. For more details, see `serviceCatalog` from the response of token issuance.

| Type | Region | Endpoint |
|---|---|---|
| network | Korea (Pangyo) Region<br>Korea (Pyeongchon) Region<br>Japan Region | https://kr1-api-network-infrastructure.nhncloudservice.com<br>https://kr2-api-network-infrastructure.nhncloudservice.com<br>https://jp1-api-network-infrastructure.nhncloudservice.com |

In each API response, you may find fields that are not specified within this guide. Those fields are for NHN Cloud internal usage, and as such refrain from using them since they may be changed without prior notice.

## VPC
### View VPC List
Returns the list of available VPCs.
```
GET /v2.0/vpcs
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.


| Name              | Type     | Format      | Required  | Description                                                                        |
|-----------------|--------|---------|-----|---------------------------------------------------------------------------|
| tokenId         | Header | String  | O   | Token ID                                                                     |
| tenant_id       | Query  | String  | -   | Tenant ID to which network to query is included                                                       |
| id              | Query  | UUID    | -   | Network ID to query                                                               |
| name            | Query  | String  | -   | Network name to query                                                               |
| router:external | Query  | Boolean | -   | Whether network is externally connected                                                        |
| shared          | Query  | Boolean | -   | Whether to share network to query                                                           |
| tenant_id       | Query  | String  | -   | Tenant ID to which network to query is included                                                       |
| sort_dir        | Query  | Enum    | -   | Sorting direction of network to query<br>Sort by the field specified by `sort_key`<br>Either **asc**, or **desc** |
| sort_key        | Query  | String  | -   | Sorting key of network to query<br>Sort in the direction as specified by `sort_dir`                                |


#### Response
| Name                   | Type   | Format      | Description                 |
|----------------------|------|---------|--------------------|
| vpcs                 | Body | Array   | List of VPC information objects       |
| vpcs.router:external | Body | Boolean | Whether VPC is externally connected  |
| vpcs.name            | Body | String  | Name of VPC to query         |
| vpcs.tenant_id       | Body | UUID    | Tenant ID to which VPC to query is included |
| vpcs.state           | Body | String  | Status of VPC to query        |
| vpcs.create_time     | Body | Date    | Created time of VPC to query     |
| vpcs.cidrv4          | Body | String  | CIDR of VPC to query     |
| vpcs.shared          | Body | Boolean | Whether to share VPC to query     |
| vpcs.id              | Body | UUID    | VPC ID to query        |

<details><summary>Example</summary>
<p>

```json
{
    "vpcs": [
        {
            "router:external": true,
            "name": "public_network",
            "tenant_id": "e873d250f2ca40b78e2c12cfbaaeb740",
            "state": "available",
            "create_time": "2020-02-27 03:16:05",
            "cidrv4": "0.0.0.0/0",
            "shared": true,
            "id": "751b8227-7b45-440a-9349-dbf829d0aba5"
        }
    ]
}
```

</p>
</details>

### View VPC
View a specified VPC.
```
GET /v2.0/vpcs/{vpcId}  
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name        | Type     | Format     | Required  | Description                  |
|-----------|--------|--------|-----|---------------------|
| vpcId     | URL    | String | O   | VPC ID              |
| tokenId   | Header | String | O   | Token ID               |

#### Response

| Name                                     | Type   | Format       | Description                             |
|----------------------------------------|------|----------|--------------------------------|
| vpc                                    | Body | Array    | VPC information object                      |
| vpc.router:external                    | Body | Boolean  | Whether VPC is externally connected              |
| vpc.routingtables                      | Body | Array    | Array of route tables for VPC to query            |
| vpc.name                               | Body | String   | Name of VPC to query                     |
| vpc.subnets                            | Body | Array    | Subnet list of VPC to query                | 
| vpc.subnets.router:external            | Body | Boolean  | Whether subnet is externally connected                  |
| vpc.subnets.name                       | Body | String   | Subnet name                        |
| vpc.subnets.enable_dhcp                | Body | Boolean  | Whether to enable DHCP of subnet               |
| vpc.subnets.tenant_id                  | Body | UUID     | Tenant ID of subnet                     |
| vpc.subnets.gateway                    | Body | String   | Gateway IP information of subnet               |
| vpc.subnets.routingtable               | Body | Object   | Routing table information of subnet                |
| vpc.subnets.routingtable.gateway_id    | Body | UUID     | Gateway ID of routing table              |
| vpc.subnets.routingtable.default_table | Body | Boolean  | Whether routing table is default routing table |
| vpc.subnets.routingtable.explicit      | Body | Boolean  | Whether routing table is explicitly connected             |
| vpc.subnets.routingtable.id            | Body | UUID     | Routing table ID                    |
| vpc.subnets.routingtable.name          | Body | String   | Routing table name                    |
| vpc.subnets.routes                     | Body | Array    | Static route to subnet                     |
| vpc.subnets.routes.subnet_id           | Body | UUID     | Subnet ID of static route                  |
| vpc.subnets.routes.tenant_id           | Body | UUID     | Tenant ID of static route                  |
| vpc.subnets.routes.mask                | Body | Integer  | Mask of static route                     |
| vpc.subnets.routes.gateway             | Body | String   | Gateway IP of static route                |
| vpc.subnets.routes.cidr                | Body | String   | CIDR of static route                   |
| vpc.subnets.routes.id                  | Body | UUID     | ID of static route                      |
| vpc.subnets.state                      | Body | String   | Subnet status                        |
| vpc.subnets.create_time                | Body | Date     | Created time for subnet                     |
| vpc.subnets.available_ip_count         | Body | Interger | Number of assignable IPs in subnet              | 
| vpc.subnets.vpc                        | Body | Object   | VPC information for subnet                    |
| vpc.subnets.vpc.shared                 | Body | Boolean  | Whether to share VPC                     |
| vpc.subnets.vpc.state                  | Body | String   | VPC status                        |
| vpc.subnets.vpc.id                     | Body | UUID     | VPC ID                        |
| vpc.subnets.vpc.cidrv4                 | Body | String   | CIDR of VPC to query                 |
| vpc.subnets.vpc.name                   | Body | String   | VPC Name                        |
| vpc.subnets.shared                     | Body | Boolean  | Whether to share subnet                     |
| vpc.subnets.id                         | Body | UUID     | Subnet ID                        |
| vpc.subnets.vpc_id                     | Body | UUID     | VPC ID of subnet                    |
| vpc.subnets.cidr                       | Body | String   | CIDR ID of subnet                   |
| vpc.tenant_id                          | Body | UUID     | Tenant ID to which VPC to query is included             |
| vpc.state                              | Body | String   | Status of VPC to query                    |
| vpc.create_time                        | Body | Date     | Created time of VPC to query                 |
| vpc.cidrv4                             | Body | String   | IPv4 of VPC to query                  |
| vpc.shared                             | Body | Boolean  | Whether to share VPC to query                 |
| vpc.id                                 | Body | UUID     | VPC ID to query                    |

<details><summary>Example</summary>
<p>

```json
{
    "vpc": {
        "router:external": true,
        "routingtables": [],
        "name": "public_network",
        "subnets": [
            {
                "router:external": true,
                "name": "public_snat_gateway",
                "enable_dhcp": true,
                "tenant_id": "e873d250f2ca40b78e2c12cfbaaeb740",
                "gateway": "100.127.64.1",
                "routes": [
                    {
                        "subnet_id": "00ef6956-f03d-406b-81c5-eb05739a578e",
                        "tenant_id": "e873d250f2ca40b78e2c12cfbaaeb740",
                        "mask": 24,
                        "gateway": "192.168.100.1",
                        "cidr": "192.168.100.0/24",
                        "id": "c7f7b8cb-e2f4-498d-9e66-c7cfef0dcdad"
                    }
                ],
                "state": "available",
                "create_time": "2020-04-16 05:10:37",
                "available_ip_count": 1,
                "vpc": {
                    "shared": true,
                    "state": "available",
                    "id": "751b8227-7b45-440a-9349-dbf829d0aba5",
                    "cidrv4": "0.0.0.0/0",
                    "name": "public_network"
                },
                "shared": true,
                "id": "00ef6956-f03d-406b-81c5-eb05739a578e",
                "vpc_id": "751b8227-7b45-440a-9349-dbf829d0aba5",
                "hidden": false,
                "cidr": "100.127.64.0/24"
            }
        ],
        "tenant_id": "e873d250f2ca40b78e2c12cfbaaeb740",
        "state": "available",
        "create_time": "2020-02-27 03:16:05",
        "cidrv4": "0.0.0.0/0",
        "shared": true,
        "id": "751b8227-7b45-440a-9349-dbf829d0aba5"
    }
}
```

</p>
</details>

### Create VPC

```
POST /v2.0/vpcs 
X-Auth-Token: {tokenId}
```

#### Request
| Name                      | Type     | Format     | Required  | Description                                   |
|-------------------------|--------|--------|-----|--------------------------------------|
| tokenId                 | Header | String | O   | Token ID                                |
| vpc                     | Body   | Object | O   | Object requesting of creating a VPC                         |
| vpc.name                | Body   | String | O   | VPC Name                               |
| vpc.cidrv4              | Body   | String | O   | CIDR of VPC                            |
| vpc.tenant_id           | Body   | UUID   |     | Tenant ID of VPC                       |
| vpc.external_network_id | Body   | UUID   |     | External network ID to be connected to VPC         |

<details><summary>Example</summary>
<p>

```json
{ 
  "vpc": { 
    "name": "vpc-test1",
    "cidrv4": "10.10.0.0/16"
  }
}
```

</p>
</details>

#### Response
| Name              | Type   | Format      | Description             |
|-----------------|------|---------|----------------|
| vpc             | Body | Array   | VPC information object      |
| vpc.name        | Body | String  | VPC Name         |
| vpc.tenant_id   | Body | UUID    | Tenant ID that VPC belongs to |
| vpc.state       | Body | String  | VPC status        |
| vpc.create_time | Body | Date    | Created time for VPC     |
| vpc.cidrv4      | Body | String  | CIDR of VPC     |
| vpc.shared      | Body | Boolean | Whether to share VPC     |
| vpc.id          | Body | UUID    | VPC ID        |
<details><summary>Example</summary>
<p>

```json
{
   "vpc": {
      "name": "vpc-test1",
      "tenant_id": "1fb0cf13afb341b699f74bbbecab2117",
      "state": "available",
      "create_time": "2023-01-17 05:14:52.037777",
      "cidrv4": "10.10.0.0/16",
      "shared": false,
      "id:": "88eb3d8b-32ef-46e4-85c4-8612ab544705"
   }
}
```

</p>
</details>

### Modify VPC
Modifies the name and CIDR of VPC.
```
PUT /v2.0/vpcs/{vpcId}
X-Auth-Token: {tokenId}
```
#### Request



| Name         | Type     | Format     | Required  | Description           |
|------------|--------|--------|-----|--------------|
| vpcId      | URL    | String | O   | VPC ID       |
| tokenId    | Header | String | O   | Token ID        |
| vpc        | Body   | Object | O   | Object requesting of creating a VPC |
| vpc.name   | Body   | String |     | VPC Name       |
| vpc.cidrv4 | Body   | String |     | CIDR of VPC    |

<details><summary>Example</summary>
<p>

```json
{
   "vpc": {
      "name": "vpc-update_test",
      "cidrv4": "10.10.0.0/20"
   }
}
```

</p>
</details>

#### Response
| Name              | Type   | Format      | Description             |
|-----------------|------|---------|----------------|
| vpc             | Body | Array   | VPC information object      |
| vpc.name        | Body | String  | VPC Name         |
| vpc.tenant_id   | Body | UUID    | Tenant ID that VPC belongs to |
| vpc.state       | Body | String  | VPC status        |
| vpc.create_time | Body | Date    | Created time for VPC     |
| vpc.cidrv4      | Body | String  | CIDR of VPC     |
| vpc.shared      | Body | Boolean | Whether to share VPC     |
| vpc.id          | Body | UUID    | VPC ID        |

<details><summary>Example</summary>
<p>

```json
{
   "vpc": {
      "name": "vpc-update_test",
      "tenant_id": "1fb0cf13afb341b699f74bbbecab2117",
      "state": "available",
      "create_time": "2023-01-17 05:14:52.037777",
      "cidrv4": "10.10.0.0/20",
      "shared": false,
      "id": "88eb3d8b-32ef-46e4-85c4-8612ab544705"
   }
}
```

</p>
</details>


### Delete VPC
Deletes a specified VPC.
```
DELETE /v2.0/vpcs/{vpcId}
X-Auth-Token: {tokenId}
```
#### Request
This API does not require a request body.

| Name      | Type     | Format     | Required  | Description     |
|---------|--------|--------|-----|--------|
| vpcId   | URL    | String | O   | VPC ID |
| tokenId | Header | String | O   | Token ID  |

#### Response
This API does not return a response body.

## VPC Subnet
### List VPC Subnets
Returns the list of available subnets.

```
GET /v2.0/vpcsubnets
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name | Type | Format | Required | Description |
|---|---|---|---|---|
| tokenId | Header | String | O | Token ID |
| id | Query | UUID | - | Subnet ID to query |
| name | Query | String | - | Subnet name to query |
| vpc_id | Query | UUID | - | VPC ID of subnet to query |
| cidr | Query | String | - | Subnet CIDR to query |
| shared | Query | Boolean | - | Whether to share subnet to query |
| sort_dir | Query | Enum | - | Sorting direction of subnet to query<br>Sort by the field specified by `sort_key`<br>Either **asc**, or **desc** |
| sort_key | Query | String | - | Sorting key of subnet to query<br>Sort in the direction as specified by `sort_dir` |


#### Response

| Name                                    | Type   | Format      | Description                        |
|---------------------------------------|------|---------|---------------------------|
| vpcsubnets                            | Body | Array   | Subnet information object                 |
| vpcsubnets.router:external            | Body | Boolean | Whether subnet is externally connected             |
| vpcsubnets.name                       | Body | String  | Subnet name                   |
| vpcsubnets.tenant_id                  | Body | UUID    | Tenant ID that subnet belongs to            |
| vpcsubnets.id                         | Body | UUID    | Subnet ID                   |
| vpcsubnets.routingtable               | Body | Object  | Routing table to which subnet is connected          |
| vpcsubnets.routingtable.gateway_id    | Body | UUID    | Gateway ID of routing table         |
| vpcsubnets.routingtable.default_table | Body | Boolean | Whether routing table is default table |
| vpcsubnets.routingtable.explicit      | Body | Boolean | Whether routing table is explicitly connected       |
| vpcsubnets.routingtable.id            | Body | UUID    | Routing table ID               |
| vpcsubnets.routingtable.name          | Body | String  | Routing table name               |
| vpcsubnets.state                      | Body | String  | Subnet status                   |
| vpcsubnets.create_time                | Body | Date    | Created time for subnet                |
| vpcsubnets.available_ip_count         | Body | Integer | Number of available IPs in subnet         |
| vpcsubnets.vpc                        | Body | Object  | VPC that subnet belongs to               |
| vpcsubnets.vpc.shared                 | Body | Boolean | Whether to share VPC                |
| vpcsubnets.vpc.state                  | Body | String  | VPC status                   |
| vpcsubnets.vpc.id                     | Body | UUID    | VPC ID                   |
| vpcsubnets.vpc.cidrv4                 | Body | String  | CIDR of VPC                |
| vpcsubnets.vpc.name                   | Body | String  | VPC Name                    |
| vpcsubnets.vpc_id                     | Body | UUID    | ID of VPC that subnet belongs to           |
| vpcsubnets.routes                     | Body | Array   | Static route to subnet                |
| vpcsubnets.routes.subnet_id           | Body | UUID    | Subnet ID of static route             |
| vpcsubnets.routes.tenant_id           | Body | UUID    | Tenant ID of static route             |
| vpcsubnets.routes.mask                | Body | Integer | Mask of static route                |
| vpcsubnets.routes.gateway             | Body | String  | Gateway IP of static route           |
| vpcsubnets.routes.cidr                | Body | String  | Destination CIDR of static route           |
| vpcsubnets.routes.id                  | Body | UUID    | ID of static route                 |
| vpcsubnets.shared                     | Body | Boolean | Whether to share subnet                |
| vpcsubnets.cidr                       | Body | String  | CIDR of subnet                |
| vpcsubnets.gateway                    | Body | String  | Gateway IP of subnet             |


<details><summary>Example</summary>
<p>

```json
{
   "vpcsubnets": [
      {
            "router:external": false,
            "name": "vpc-subnet-2",
            "tenant_id": "1fb0cf13afb341b699f74bbbecab2117",
            "state": "available",
            "id": "8136b0e0-6360-4392-8947-48febf857cad",
            "routingtable": {
                "gateway_id": "4d6f3b31-f58d-4b39-9787-c1c39c6282ac",
                "default_table": true,
                "explicit": false,
                "id": "01ea946c-08d0-44fe-8e65-3c2cddfaa527",
                "name": "vpc-8a725559-671f"
            },
            "create_time": "2023-01-16 07:24:01",
            "available_ip_count": 251,
            "vpc": {
                "shared": false,
                "state": "available",
                "id": "8a725559-671f-486f-96f0-8d77a21f6394",
                "cidrv4": "192.168.0.0/16",
                "name": "hcpak-network"
            },
            "vpc_id": "8a725559-671f-486f-96f0-8d77a21f6394",
            "routes": [
                {
                    "subnet_id": "8136b0e0-6360-4392-8947-48febf857cad",
                    "tenant_id": "1fb0cf13afb341b699f74bbbecab2117",
                    "mask": 24,
                    "gateway": "192.168.100.1",
                    "cidr": "192.168.100.0/24",
                    "id": "c7f7b8cb-e2f4-498d-9e66-c7cfef0dcdad"
                }
            ],
            "shared": false,
            "cidr": "192.168.1.0/24",
            "gateway": "192.168.1.1"
        }
   ]
}
```

</p>
</details>

### View VPC Subnet
Views the specified subnet.
```
GET /v2.0/vpcsubnets/{subnetId}  
X-Auth-Token: {tokenId}
```

#### Request
This API does not require a request body.

| Name       | Type     | Format     | Required  | Description     |
|----------|--------|--------|-----|--------|
| subnetId | URL    | UUID   | O   | Subnet ID |
| tokenId  | Header | String | O   | Token ID  |


#### Response

| Name                                   | Type   | Format      | Description                        |
|--------------------------------------|------|---------|---------------------------|
| vpcsubnet                            | Body | Array   | Subnet information object                 |
| vpcsubnet.router:external            | Body | Boolean | Whether subnet is externally connected             |
| vpcsubnet.name                       | Body | String  | Subnet name                   |
| vpcsubnet.tenant_id                  | Body | UUID    | Tenant ID that subnet belongs to            |
| vpcsubnet.id                         | Body | UUID    | Subnet ID                   |
| vpcsubnet.routingtable               | Body | Object  | Routing table to which subnet is connected          |
| vpcsubnet.routingtable.gateway_id    | Body | UUID    | Gateway ID of routing table         |
| vpcsubnet.routingtable.default_table | Body | Boolean | Whether routing table is default table |
| vpcsubnet.routingtable.explicit      | Body | Boolean | Whether routing table is explicitly connected       |
| vpcsubnet.routingtable.id            | Body | UUID    | Routing table ID               |
| vpcsubnet.routingtable.name          | Body | String  | Routing table name               |
| vpcsubnet.state                      | Body | String  | Subnet status                   |
| vpcsubnet.create_time                | Body | Date    | Created time for subnet                |
| vpcsubnet.available_ip_count         | Body | Integer | Number of available IPs in subnet         |
| vpcsubnet.vpc                        | Body | Object  | VPC that subnet belongs to               |
| vpcsubnet.vpc.shared                 | Body | Boolean | Whether to share VPC                |
| vpcsubnet.vpc.state                  | Body | String  | VPC status                   |
| vpcsubnet.vpc.id                     | Body | UUID    | VPC ID                   |
| vpcsubnet.vpc.cidrv4                 | Body | String  | CIDR of VPC                |
| vpcsubnet.vpc.name                   | Body | String  | VPC Name                    |
| vpcsubnet.vpc_id                     | Body | UUID    | ID of VPC that subnet belongs to           |
| vpcsubnet.routes                     | Body | Array   | Static route to subnet                |
| vpcsubnet.routes.subnet_id           | Body | UUID    | Subnet ID of static route             |
| vpcsubnet.routes.tenant_id           | Body | UUID    | Tenant ID of static route             |
| vpcsubnet.routes.mask                | Body | Integer | Mask of static route                |
| vpcsubnet.routes.gateway             | Body | String  | Gateway IP of static route           |
| vpcsubnet.routes.cidr                | Body | String  | Destination CIDR of static route           |
| vpcsubnet.routes.id                  | Body | UUID    | ID of static route                 |
| vpcsubnet.shared                     | Body | Boolean | Whether to share subnet                |
| vpcsubnet.cidr                       | Body | String  | CIDR of subnet                |
| vpcsubnet.gateway                    | Body | String  | Gateway IP of subnet             |


<details><summary>Example</summary>
<p>

```json
{
    "vpcsubnet": {
        "router:external": false,
        "name": "vpc-subnet-2",
        "tenant_id": "1fb0cf13afb341b699f74bbbecab2117",
        "state": "available",
        "id": "8136b0e0-6360-4392-8947-48febf857cad",
        "routingtable": {
            "gateway_id": "4d6f3b31-f58d-4b39-9787-c1c39c6282ac",
            "default_table": true,
            "explicit": false,
            "id": "01ea946c-08d0-44fe-8e65-3c2cddfaa527",
            "name": "vpc-8a725559-671f"
        },
        "create_time": "2023-01-16 07:24:01",
        "available_ip_count": 251,
        "vpc": {
            "shared": false,
            "state": "available",
            "id": "8a725559-671f-486f-96f0-8d77a21f6394",
            "cidrv4": "192.168.0.0/16",
            "name": "hcpak-network"
        },
        "vpc_id": "8a725559-671f-486f-96f0-8d77a21f6394",
        "routes": [
            {
                "subnet_id": "8136b0e0-6360-4392-8947-48febf857cad",
                "tenant_id": "1fb0cf13afb341b699f74bbbecab2117",
                "mask": 24,
                "gateway": "192.168.100.1",
                "cidr": "192.168.100.0/24",
                "id": "c7f7b8cb-e2f4-498d-9e66-c7cfef0dcdad"
            }
        ],
        "shared": false,
        "cidr": "192.168.1.0/24",
        "gateway": "192.168.1.1"
    }
}
```

</p>
</details>

### Create VPC Subnet
Creates a new subnet.
```
POST /v2.0/vpcsubnets  
X-Auth-Token: {tokenId}
```
#### Request

| Name                  | Type     | Format     | Required  | Description              |
|---------------------|--------|--------|-----|-----------------|
| tokenId             | Header | String | O   | Token ID           |
| vpcsubnet           | Body   | Object | O   | Subnet information object       |
| vpcsubnet.vpc_id    | Body   | UUID   | O   | VPC ID to which subnet is assigned |
| vpcsubnet.cidr      | Body   | String | O   | CIDR of subnet      |
| vpcsubnet.name      | Body   | String | O   | Subnet name         |
| vpcsubnet.tenant_id | Body   | UUID   |     | Tenant ID to which subnet is assigned |

<details><summary>Example</summary>
<p>

```json
{
   "vpcsubnet": {
      "vpc_id": "b9f8e7b1-45e8-4eb5-95b2-7256dc60f7ff",
      "cidr": "10.3.0.0/16",
      "name": "vpc-subnet-1"
   }
}
```

</p>
</details>

#### Response

| Name                    | Type   | Format      | Description                |
|-----------------------|------|---------|-------------------|
| vpcsubnet             | Body | Array   | Subnet information object         |
| vpcsubnet.name        | Body | String  | Subnet name           |
| vpcsubnet.tenant_id   | Body | String  | Tenant ID that subnet belongs to    |
| vpcsubnet.id          | Body | UUID    | Subnet ID           |
| vpcsubnet.state       | Body | String  | Subnet status           |
| vpcsubnet.create_time | Body | Date    | Created time for subnet        |
| vpcsubnet.vpc_id      | Body | String  | ID of VPC that subnet belongs to   |
| vpcsubnet.shared      | Body | Boolean | Whether to share subnet        |
| vpcsubnet.cidr        | Body | String  | CIDR of subnet        |
| vpcsubnet.gateway     | Body | String  | Gateway IP of subnet |

<details><summary>Example</summary>
<p>

```json
{
   "vpcsubnet": {
      "name": "vpc-subnet-1",
      "tenant_id": "1fb0cf13afb341b699f74bbbecab2117",
      "id": "bc6c7035-f9a7-4eea-a6c7-14c3036aa912",
      "state": "available",
      "create_time": "2023-01-17 05:39:28",
      "vpc_id": "b9f8e7b1-45e8-4eb5-95b2-7256dc60f7ff",
      "shared": false,
      "cidr": "10.3.0.0/16",
      "gateway": "10.3.0.1"
   }
}
```

</p>
</details>

### Modify VPC Subnet
Modifies the subnet name.
```
PUT /v2.0/vpcsubnets/{subnetId}  
X-Auth-Token: {tokenId}
```
#### Request
| Name             | Type     | Format     | Required  | Description        |
|----------------|--------|--------|-----|-----------|
| subnetId       | URL    | UUID   | O   | Subnet ID    |
| tokenId        | Header | String | O   | Token ID     |
| vpcsubnet      | Body   | Object | O   | Subnet information object |
| vpcsubnet.name | Body   | String | O   | Subnet name   |

<details><summary>Example</summary>
<p>

```json
{
   "vpcsubnet": {
      "name": "subnet_update_test"
   }
}
```

</p>
</details>

#### Response

| Name                    | Type   | Format      | Description                |
|-----------------------|------|---------|-------------------|
| vpcsubnet             | Body | Array   | Subnet information object         |
| vpcsubnet.name        | Body | String  | Subnet name           |
| vpcsubnet.tenant_id   | Body | String  | Tenant ID that subnet belongs to    |
| vpcsubnet.id          | Body | UUID    | Subnet ID           |
| vpcsubnet.state       | Body | String  | Subnet status           |
| vpcsubnet.create_time | Body | Date    | Created time for subnet        |
| vpcsubnet.vpc_id      | Body | String  | ID of VPC that subnet belongs to   |
| vpcsubnet.shared      | Body | Boolean | Whether to share subnet        |
| vpcsubnet.cidr        | Body | String  | CIDR of subnet        |
| vpcsubnet.gateway     | Body | String  | Gateway IP of subnet |

<details><summary>Example</summary>
<p>

```json
{
   "vpcsubnet": {
      "name": "subnet_update_test",
      "tenant_id": "1fb0cf13afb341b699f74bbbecab2117",
      "id": "bc6c7035-f9a7-4eea-a6c7-14c3036aa912",
      "state": "available",
      "create_time": "2023-01-17 05:39:28",
      "vpc_id": "b9f8e7b1-45e8-4eb5-95b2-7256dc60f7ff",
      "shared": false,
      "cidr": "10.3.0.0/16",
      "gateway": "10.3.0.1"
   }
}
```

</p>
</details>

### Connect VPC Subnet to Routing Table
Connects a subnet to a specific routing table.
```
PUT /v2.0/vpcsubnets/{subnetId}/attach_routingtable
X-Auth-Token: {tokenId}
```
#### Request
| Name       | Type     | Format     | Required  | Description     |
|----------|--------|--------|-----|--------|
| subnetId | URL    | UUID   | O   | Subnet ID |
| tokenId  | Header | String | O   | Token ID  |
| routingtable_id | Body | UUID | O   | Routing table ID to connect |

<details><summary>Example</summary>
<p>

```json
{
   "routingtable_id": "c577a618-1acb-444d-a82e-aeb4c6fce01e"
}
```

</p>
</details>

#### Response

| Name                                   | Type   | Format      | Description                        |
|--------------------------------------|------|---------|---------------------------|
| vpcsubnet                            | Body | Array   | Subnet information object                 |
| vpcsubnet.router:external            | Body | Boolean | Whether subnet is externally connected             |
| vpcsubnet.name                       | Body | String  | Subnet name                   |
| vpcsubnet.tenant_id                  | Body | UUID    | Tenant ID that subnet belongs to            |
| vpcsubnet.id                         | Body | UUID    | Subnet ID                   |
| vpcsubnet.routingtable               | Body | Object  | Routing table to which subnet is connected          |
| vpcsubnet.routingtable.gateway_id    | Body | UUID    | Gateway ID of routing table         |
| vpcsubnet.routingtable.default_table | Body | Boolean | Whether routing table is default table |
| vpcsubnet.routingtable.explicit      | Body | Boolean | Whether routing table is explicitly connected       |
| vpcsubnet.routingtable.id            | Body | UUID    | Routing table ID               |
| vpcsubnet.routingtable.name          | Body | String  | Routing table name               |
| vpcsubnet.state                      | Body | String  | Subnet status                   |
| vpcsubnet.create_time                | Body | Date    | Created time for subnet                |
| vpcsubnet.available_ip_count         | Body | Integer | Number of available IPs in subnet         |
| vpcsubnet.vpc                        | Body | Object  | VPC that subnet belongs to               |
| vpcsubnet.vpc.shared                 | Body | Boolean | Whether to share VPC                |
| vpcsubnet.vpc.state                  | Body | String  | VPC status                   |
| vpcsubnet.vpc.id                     | Body | UUID    | VPC ID                   |
| vpcsubnet.vpc.cidrv4                 | Body | String  | CIDR of VPC                |
| vpcsubnet.vpc.name                   | Body | String  | VPC Name                    |
| vpcsubnet.vpc_id                     | Body | UUID    | ID of VPC that subnet belongs to           |
| vpcsubnet.routes                     | Body | Array   | Static route to subnet                |
| vpcsubnet.routes.subnet_id           | Body | UUID    | Subnet ID of static route             |
| vpcsubnet.routes.tenant_id           | Body | UUID    | Tenant ID of static route             |
| vpcsubnet.routes.mask                | Body | Integer | Mask of static route                |
| vpcsubnet.routes.gateway             | Body | String  | Gateway IP of static route           |
| vpcsubnet.routes.cidr                | Body | String  | Destination CIDR of static route           |
| vpcsubnet.routes.id                  | Body | UUID    | ID of static route                 |
| vpcsubnet.shared                     | Body | Boolean | Whether to share subnet                |
| vpcsubnet.cidr                       | Body | String  | CIDR of subnet                |
| vpcsubnet.gateway                    | Body | String  | Gateway IP of subnet             |



<details><summary>Example</summary>
<p>

```json
{
   "vpcsubnet": {
      "router:external": false,
      "name": "hcpak-subnet-2",
      "enable_dhcp": true,
      "tenant_id": "1fb0cf13afb341b699f74bbbecab2117",
      "routingtable": {
         "default_table": false,
         "explicit": true,
         "id": "4c05e594-fa28-4651-8235-7c6d3757360f",
         "name": "test"
      },
      "gateway": "10.1.0.1",
      "routes": [],
      "state": "available",
      "create_time": "2023-01-16 08:04:45",
      "available_ip_count": 65532,
      "vpc": {
         "shared": false,
         "state": "available",
         "id": "b9f8e7b1-45e8-4eb5-95b2-7256dc60f7ff",
         "cidrv4": "10.0.0.0/8",
         "name": "hcpak-network2"
      },
      "shared": false,
      "id": "ab33b9bc-c447-48fa-a6a0-bad40bc952f3",
      "vpc_id": "b9f8e7b1-45e8-4eb5-95b2-7256dc60f7ff",
      "hidden": false,
      "cidr": "10.1.0.0/16"
   }
}
```

</p>
</details>

### Detach VPC Subnet from Routing Table
Detach explicit connection between subnet and routing table

```
PUT /v2.0/vpcsubnets/{subnetId}/detach_routingtable
X-Auth-Token: {tokenId}
```
#### Request
This API does not require a request body.

| Name       | Type     | Format     | Required  | Description     |
|----------|--------|--------|-----|--------|
| subnetId | URL    | UUID   | O   | Subnet ID |
| tokenId  | Header | String | O   | Token ID  |

#### Response

| Name                                   | Type   | Format      | Description                       |
|--------------------------------------|------|---------|--------------------------|
| vpcsubnet                            | Body | Array   | Subnet information object                |
| vpcsubnet.router:external            | Body | Boolean | Whether subnet is externally connected            |
| vpcsubnet.name                       | Body | String  | Subnet name                  |
| vpcsubnet.enable_dhcp                | Body | Boolean | Whether to enable DHCP of subnet         |
| vpcsubnet.tenant_id                  | Body | UUID    | Tenant ID that subnet belongs to           |
| vpcsubnet.routingtable               | Body | Object  | Routing table of subnet             |
| vpcsubnet.routingtable.gateway_id    | Body | UUID    | Gateway ID of routing table        |
| vpcsubnet.routingtable.default_table | Body | Boolean | Whether routing table is default table |
| vpcsubnet.routingtable.explicit      | Body | Boolean | Whether routing table is explicitly connected       |
| vpcsubnet.routingtable.id            | Body | UUID    | Routing table ID              |
| vpcsubnet.routingtable.name          | Body | String  | Routing table name              |
| vpcsubnet.gateway                    | Body | String  | Gateway IP connected to subnet        |
| vpcsubnet.routes                     | Body | Array   | Static route to subnet               |
| vpcsubnet.routes.subnet_id           | Body | UUID    | Subnet ID of static route            |
| vpcsubnet.routes.tenant_id           | Body | UUID    | Tenant ID of static route            |
| vpcsubnet.routes.mask                | Body | Integer | Mask of static route               |
| vpcsubnet.routes.gateway             | Body | String  | Gateway IP of static route          |
| vpcsubnet.routes.cidr                | Body | String  | CIDR of static route             |
| vpcsubnet.routes.id                  | Body | UUID    | ID of static route                |
| vpcsubnet.state                      | Body | String  | Subnet status                  |
| vpcsubnet.create_time                | Body | Date    | Created time for subnet               |
| vpcsubnet.available_ip_count         | Body | Integer | Number of available IPs that subnet can use        |
| vpcsubnet.vpc                        | Body | Object  | VPC that subnet belongs to              |
| vpcsubnet.vpc.shared                 | Body | Boolean | Whether to share VPC               |
| vpcsubnet.vpc.state                  | Body | String  | VPC status                  |
| vpcsubnet.vpc.id                     | Body | UUID    | VPC ID                  |
| vpcsubnet.vpc.cidrv4                 | Body | String  | CIDR of VPC               |
| vpcsubnet.vpc.name                   | Body | String  | VPC Name                   |
| vpcsubnet.shared                     | Body | Boolean | Whether to share subnet               |
| vpcsubnet.id                         | Body | UUID    | Subnet ID                  |
| vpcsubnet.vpc_id                     | Body | UUID    | ID of VPC that subnet belongs to          |
| vpcsubnet.cidr                       | Body | String  | CIDR of subnet               |


<details><summary>Example</summary>
<p>

```json
{
   "vpcsubnet": {
      "router:external": false,
      "name": "hcpak-subnet-2",
      "enable_dhcp": true,
      "tenant_id": "1fb0cf13afb341b699f74bbbecab2117",
      "routingtable": {
         "gateway_id": "76fc0ab6-ca7a-46e8-b3e3-c1b37658e836",
         "default_table": true,
         "explicit": false,
         "id": "64b21edd-8cc7-4e78-a868-fd65091fee3c",
         "name": "vpc-b9f8e7b1-45e8"
      },
      "gateway": "10.1.0.1",
      "routes": [],
      "state": "available",
      "create_time": "2023-01-16 08:04:45",
      "available_ip_count": 65531,
      "vpc": {
         "shared": false,
         "state": "available",
         "id": "b9f8e7b1-45e8-4eb5-95b2-7256dc60f7ff",
         "cidrv4": "10.0.0.0/8",
         "name": "hcpak-network2"
      },
      "shared": false,
      "id": "ab33b9bc-c447-48fa-a6a0-bad40bc952f3",
      "vpc_id": "b9f8e7b1-45e8-4eb5-95b2-7256dc60f7ff",
      "hidden": false,
      "cidr": "10.1.0.0/16"
   }
}
```

</p>
</details>

### Delete VPC Subnet

Deletes a specified subnet.

```
DELETE /v2.0/vpcsubnets/{subnetId}  
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name       | Type     | Format     | Required  | Description     |
|----------|--------|--------|-----|--------|
| subnetId | URL    | UUID   | O   | Subnet ID |
| tokenId  | Header | String | O   | Token ID  |

#### Response
This API does not return a response body.


## Routing Table

### View Routing Table List

Returns the list of available routing tables.

```
GET /v2.0/routingtables
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name       | Type     | Format     | Required  | Description     |
|----------|--------|--------|-----|--------|
| tokenId | Header | String | O | Token ID |
| tenant_id | Query | String | - | Tenant ID to which routing table to query is included |
| id | Query | UUID | - | Routing table ID to query |
| name | Query | String | - | Routing table name to query |
| default_table | Query | Boolean | - | Whether routing table to query is default table |
| gateway_id | Query | UUID | - | Internet gateway ID associated with routing table to query |
| distributed | Query | Boolean | - | Routing method of routing table to query<br>`true`: Distributed, `false`: Centralized | 
| detail | Query | Boolean | - | Whether or not to include details of the routing table to be queried |
| sort_dir | Query  | Enum    | -   | Sorting direction of network to query<br>Sort by the field specified by `sort_key`<br>Either **asc**, or **desc** |
| sort_key | Query  | String  | -   | Sorting key of network to query<br>Sort in the direction as specified by `sort_dir` |

#### Response

##### When there is no **detail** query parameter or when the value is `false`

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| routingtables | Body | Array | List of routing table information objects |
| routingtables.id | Body | UUID | Routing table ID |
| routingtables.name | Body | String | Routing table name |
| routingtables.default_table | Body | Boolean | Whether routing table is default table |
| routingtables.distributed | Body | Boolean | Routing method of routing table to query<br>`true`: Distributed, `false`: Centralized |
| routingtables.gateway_id | Body | UUID | The ID of the Internet gateway if the Internet gateway of distributed type is associated with the routing table. |
| routingtables.tenant_id | Body | String | Tenant ID to which routing table is included |
| routingtables.state | Body | String | Status of the routing table. Currently, only the `available` status exists |
| routingtable.create_time | Date | Routing table creation time |

<details><summary>Example</summary>
<p>

```json
{
  "routingtables": [
    {
      "gateway_id": "e0e51d26-f8e8-4643-9b1a-01562db00949",
      "name": "vpc-162de82d-7301",
      "tenant_id": "130f20670ac34949b64b10ad8a5989c8",
      "distributed": false,
      "state": "available",
      "default_table": true,
      "create_time": "2022-08-17 06:55:17",
      "id": "51848e19-f4ae-489a-9428-71927b97f3b5"
    }
  ]
}
```

</p>
</details>

##### When the **detail** query parameter is `true`

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| routingtables | Body | Array | List of routing table information objects |
| routingtables.id | Body | UUID | Routing table ID |
| routingtables.name | Body | String | Routing table name |
| routingtables.default_table | Body | Boolean | Whether routing table is default table |
| routingtables.distributed | Body | Boolean | Routing method of routing table to query<br>`true`: Distributed, `false`: Centralized |
| routingtables.gateway_id | Body | UUID | The ID of the internet gateway when the gateway is associated with routing table |
| routingtables.tenant_id | Body | String | Tenant ID to which routing table is included |
| routingtables.state | Body | String | Status of the routing table. Currently, only the `available` status exists |
| routingtables.vpcs | Body | Array | List of VPC information objects to which routing table belongs |
| routingtables.vpcs.id | Body | UUID | VPC ID to which routing table is included |
| routingtables.vpcs.name | Body | String | VPC ID to which routing table is included |
| routingtables.subnets | Body | Array | List of subnet information objects associated with routing table | 
| routingtables.subnets.id | Body | UUID | Subnet ID linked to routing table | 
| routingtables.subnet.name | Body | UUID | Subnet name associated with routing table |
| routingtables.create_time | Date | Routing table creation time |

<details><summary>Example</summary>
<p>

```json
{
  "routingtables": [
    {
      "gateway_id": "e0e51d26-f8e8-4643-9b1a-01562db00949",
      "subnets": [
        {
          "name": "Default Network",
          "id": "2f8919a1-d06b-480f-8e26-87c7c9eee16f"
        }
      ],
      "name": "vpc-162de82d-7301",
      "vpcs": [
        {
          "name": "Default Network",
          "id": "162de82d-7301-4141-ae7a-36c3dc9a63f8"
        }
      ],
      "tenant_id": "130f20670ac34949b64b10ad8a5989c8",
      "distributed": false,
      "state": "available",
      "default_table": true,
      "create_time": "2022-08-17 06:55:17",
      "id": "51848e19-f4ae-489a-9428-71927b97f3b5"
    }
  ]
}
```

</p>
</details>

### View Routing Table

Views the specified routing table.

```
GET /v2.0/routingtables/{routingtableId}
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| routingtableId | URL | UUID | O | Routing table ID to query |
| tokenId | Header | String | O | Token ID |

#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| routingtable | Body | Object | Port information object |
| routingtable.id | Body | UUID | Routing table ID |
| routingtable.name | Body | String | Routing table name |
| routingtable.default_table | Body | Boolean | Whether routing table is default table |
| routingtable.distributed | Body | Boolean | Routing method of routing table to query<br>`true`: Distributed, `false`: Centralized |
| routingtable.gateway_id | Body | UUID | The ID of the internet gateway when the gateway is associated with routing table |
| routingtable.tenant_id | Body | String | Tenant ID to which routing table is included |
| routingtable.state | Body | String | Status of the routing table. Currently, only the `available` status exists |
| routingtable.vpcs | Body | Array | List of VPC information objects to which routing table belongs |
| routingtable.vpcs.id | Body | UUID | VPC ID to which routing table is included |
| routingtable.subnets | Body | Array | List of subnet information objects associated with routing table | 
| routingtable.subnets.id | Body | UUID | Subnet ID linked to routing table | 
| routingtable.routes | Body | Array | List of route information objects set to routing table |
| routingtable.routes.id | Body | UUID | Route ID |
| routingtable.routes.cidr | Body | String | Route destination CIDR |
| routingtable.routes.mask | Body | String | Netmask of route destination CIDR |
| routingtable.routes.gateway | Body | String | Route gateway IP |
| routingtable.routes.gateway_id | Body | String | For routes to an internet gateway, the internet gateway ID |
| routingtable.routes.routingtable_id | Body | String | Table ID to which route is included |
| routingtable.routes.tenant_id | Body | String | Tenant ID that route belongs to |
| routingtable.create_time | Date | Routing table creation time |

<details><summary>Example</summary>
<p>

```json
{
  "routingtable": {
    "gateway_id": "e0e51d26-f8e8-4643-9b1a-01562db00949",
    "subnets": [
      "2f8919a1-d06b-480f-8e26-87c7c9eee16f"
    ],
    "name": "vpc-162de82d-7301",
    "vpcs": [
      "162de82d-7301-4141-ae7a-36c3dc9a63f8"
    ],
    "tenant_id": "130f20670ac34949b64b10ad8a5989c8",
    "distributed": false,
    "state": "available",
    "default_table": true,
    "create_time": "2022-08-17 06:55:17",
    "routes": [
      {
        "tenant_id": "130f20670ac34949b64b10ad8a5989c8",
        "mask": 16,
        "id": "520336f6-8920-4ff9-a7c5-fe9db27f026a",
        "gateway": "local",
        "routingtable_id": "51848e19-f4ae-489a-9428-71927b97f3b5",
        "cidr": "192.168.0.0/16"
      },
      {
        "gateway_id": "e0e51d26-f8e8-4643-9b1a-01562db00949",
        "tenant_id": "130f20670ac34949b64b10ad8a5989c8",
        "mask": 0,
        "id": "ddd375f5-5b6b-4e1d-83d0-9d1444224db2",
        "gateway": "192.168.120.82",
        "routingtable_id": "51848e19-f4ae-489a-9428-71927b97f3b5",
        "cidr": "0.0.0.0/0"
      }
    ],
    "id": "51848e19-f4ae-489a-9428-71927b97f3b5"
  }
}
```

</p>
</details>

### Create Routing Table

Create a new routing table.

```
POST /v2.0/routingtables
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | Token ID |
| routingtable | Body | Object | O | Routing table creation request object |
| routingtable.name | Body | String | O | Routing table name |
| routingtable.vpc_id | Body | UUID | O | VPC ID to which the routing table belongs |
| routingtable.distributed | Body | Boolean | - | Routing method of routing table<br>`true`: Distributed, `false`: Centralized (default is `true`) |

<details><summary>Example</summary>
<p>

```json
{
  "routingtable": {
    "name": "second_routing_table",
    "vpc_id": "e3c11abd-41d9-4f7d-a8c1-a5e62d65bce4",
    "distributed": "true"
  }
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| routingtable | Body | Object | Created routing table information object |
| id | Body | UUID | Routing table ID |
| name | Body | String | Routing table name |
| default_table | Body | Boolean | Whether routing table is default table |
| distributed | Body | Boolean | Routing method of routing table<br>`true`: Distributed, `false`: Centralized |
| state | Body | String | Status of the routing table. Currently, only the `available` status exists |
| tenant_id | Body | String | Tenant ID to which routing table is included |
| create_time | Body | Date | Routing table creation time |

<details><summary>Example</summary>
<p>

```json
{
  "routingtable": {
    "default_table": false,
    "name": "second_routing_table",
    "tenant_id": "8189ec9dc39c4a418359603e2b84a754",
    "distributed": false,
    "state": "available",
    "create_time": "2023-07-25 05:46:40",
    "id": "9abb4a7c-3609-4e1c-85e3-27f6ad127ee8"
  }
}
```

</p>
</details>

### Modify Routing Table

Modifies the routing table information. You can change the name of the routing table and the routing method (distributed/centralized).

```
PUT /v2.0/routingtables/{routingtableId}
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| routingtableId | URL | UUID | O | Routing table ID to modify |
| tokenId | Header | String | O | Token ID |
| routingtable | Body | Object | O | Routing table information object |
| routingtable.name | Body | String | - | Routing table name to change |
| routingtable.distributed | Body | Boolean | - | Routing method of routing table to change<br>`true`: Distributed, `false`: Centralized | 


<details><summary>Example</summary>
<p>

```json
{
  "routingtable": {
    "distributed": false, 
    "name": "centralized"
  }
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| routingtable | Body | Object | Created routing table information object |
| id | Body | UUID | Routing table ID |
| name | Body | String | Routing table name |
| default_table | Body | Boolean | Whether routing table is default table |
| distributed | Body | Boolean | Routing method of routing table<br>`true`: Distributed, `false`: Centralized |
| state | Body | String | Status of the routing table. Currently, only the `available` status exists |
| tenant_id | Body | String | Tenant ID to which routing table is included |
| create_time | Body | Date | Routing table creation time |

<details><summary>Example</summary>
<p>

```json
{
  "routingtable": {
    "default_table": false,
    "name": "centralized",
    "tenant_id": "8189ec9dc39c4a418359603e2b84a754",
    "distributed": false,
    "state": "available",
    "create_time": "2023-07-25 05:46:40",
    "id": "9abb4a7c-3609-4e1c-85e3-27f6ad127ee8"
  }
}
```

</p>
</details>


### Associate Internet Gateway with Routing Table

Associates an internet gateway with routing table.

```
PUT /v2.0/routingtables/{routingtableId}/attach_gateway
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| routingtableId | URL | UUID | O | Routing table ID to modify |
| tokenId | Header | String | O | Token ID |
| gateway_id | Body | UUID | O | Internet gateway ID to be associated with routing table |

<details><summary>Example</summary>
<p>

```json
{
  "gateway_id": "615d1cb1-fe54-4505-8a39-35faa6c868cd"
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| routingtable | Body | Object | Port information object |
| routingtable.id | Body | UUID | Routing table ID |
| routingtable.name | Body | String | Routing table name |
| routingtable.default_table | Body | Boolean | Whether routing table is default table |
| routingtable.distributed | Body | Boolean | Routing method of routing table to query<br>`true`: Distributed, `false`: Centralized |
| routingtable.gateway_id | Body | UUID | The ID of the internet gateway when the gateway is associated with routing table |
| routingtable.tenant_id | Body | String | Tenant ID to which routing table is included |
| routingtable.state | Body | String | Status of the routing table. Currently, only the `available` status exists |
| routingtable.vpcs | Body | Array | List of VPC information objects to which routing table belongs |
| routingtable.vpcs.id | Body | UUID | VPC ID to which routing table is included |
| routingtable.subnets | Body | Array | List of subnet information objects associated with routing table | 
| routingtable.subnets.id | Body | UUID | Subnet ID linked to routing table | 
| routingtable.routes | Body | Array | List of route information objects set to routing table |
| routingtable.routes.id | Body | UUID | Route ID |
| routingtable.routes.cidr | Body | String | Route destination CIDR |
| routingtable.routes.mask | Body | String | Netmask of route destination CIDR |
| routingtable.routes.gateway | Body | String | Route gateway IP |
| routingtable.routes.gateway_id | Body | String | For routes to an internet gateway, the internet gateway ID |
| routingtable.routes.routingtable_id | Body | String | Table ID to which route is included |
| routingtable.routes.tenant_id | Body | String | Tenant ID that route belongs to |
| routingtable.create_time | Date | Routing table creation time |

<details><summary>Example</summary>
<p>

```json
{
  "routingtable": {
    "gateway_id": "615d1cb1-fe54-4505-8a39-35faa6c868cd",
    "subnets": [],
    "name": "second_routingtable",
    "vpcs": [
      "e3c11abd-41d9-4f7d-a8c1-a5e62d65bce4"
    ],
    "tenant_id": "8189ec9dc39c4a418359603e2b84a754",
    "activated": true,
    "distributed": true,
    "state": "available",
    "default_table": false,
    "create_time": "2023-07-25 05:46:40",
    "routes": [
      {
        "tenant_id": "8189ec9dc39c4a418359603e2b84a754",
        "mask": 24,
        "id": "4e1cfa8d-7947-4aa8-b0dd-2012397e083f",
        "gateway": "local",
        "routingtable_id": "9abb4a7c-3609-4e1c-85e3-27f6ad127ee8",
        "cidr": "10.0.20.0/24",
        "hidden": false
      },
      {
        "gateway_id": "615d1cb1-fe54-4505-8a39-35faa6c868cd",
        "tenant_id": "8189ec9dc39c4a418359603e2b84a754",
        "mask": 0,
        "id": "06c233a7-d1c3-4e0e-83bc-b497420f346b",
        "gateway": "100.127.65.120",
        "routingtable_id": "9abb4a7c-3609-4e1c-85e3-27f6ad127ee8",
        "cidr": "0.0.0.0/0",
        "hidden": false
      }
    ],
    "id": "9abb4a7c-3609-4e1c-85e3-27f6ad127ee8"
  }
}
```

</p>
</details>

### Disassociate Internet Gateway from Routing Table

Disassociates an internet gateway from the routing table.

```
PUT /v2.0/routingtables/{routingtableId}/detach_gateway
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| routingtableId | URL | UUID | O | Routing table ID to modify |
| tokenId | Header | String | O | Token ID |


#### Response
| Name | Type | Format | Description |
| --- | --- | --- | --- |
| routingtable | Body | Object | Port information object |
| routingtable.id | Body | UUID | Routing table ID |
| routingtable.name | Body | String | Routing table name |
| routingtable.default_table | Body | Boolean | Whether routing table is default table |
| routingtable.distributed | Body | Boolean | Routing method of routing table to query<br>`true`: Distributed, `false`: Centralized |
| routingtable.tenant_id | Body | String | Tenant ID to which routing table is included |
| routingtable.state | Body | String | Status of the routing table. Currently, only the `available` status exists |
| routingtable.vpcs | Body | Array | List of VPC information objects to which routing table belongs |
| routingtable.vpcs.id | Body | UUID | VPC ID to which routing table is included |
| routingtable.subnets | Body | Array | List of subnet information objects associated with routing table | 
| routingtable.subnets.id | Body | UUID | Subnet ID linked to routing table | 
| routingtable.routes | Body | Array | List of route information objects set to routing table |
| routingtable.routes.id | Body | UUID | Route ID |
| routingtable.routes.cidr | Body | String | Route destination CIDR |
| routingtable.routes.mask | Body | String | Netmask of route destination CIDR |
| routingtable.routes.gateway | Body | String | Route gateway IP |
| routingtable.routes.gateway_id | Body | String | For routes to an internet gateway, the internet gateway ID |
| routingtable.routes.routingtable_id | Body | String | Table ID to which route is included |
| routingtable.routes.tenant_id | Body | String | Tenant ID that route belongs to |
| routingtable.create_time | Date | Routing table creation time |

<details><summary>Example</summary>
<p>

```json
{
  "routingtable": {
    "subnets": [],
    "name": "second_routingtable",
    "vpcs": [
      "e3c11abd-41d9-4f7d-a8c1-a5e62d65bce4"
    ],
    "tenant_id": "8189ec9dc39c4a418359603e2b84a754",
    "activated": true,
    "distributed": true,
    "state": "available",
    "default_table": false,
    "create_time": "2023-07-25 05:46:40",
    "routes": [
      {
        "tenant_id": "8189ec9dc39c4a418359603e2b84a754",
        "mask": 24,
        "id": "4e1cfa8d-7947-4aa8-b0dd-2012397e083f",
        "gateway": "local",
        "routingtable_id": "9abb4a7c-3609-4e1c-85e3-27f6ad127ee8",
        "cidr": "10.0.20.0/24",
        "hidden": false
      }
    ],
    "id": "9abb4a7c-3609-4e1c-85e3-27f6ad127ee8"
  }
}
```

</p>
</details>

### Designate Routing Table as Default Routing Table

Designates a routing table as default routing table.

```
PUT /v2.0/routingtables/{routingtableId}/set_as_default
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| routingtableId | URL | UUID | O | Routing table ID to modify |
| tokenId | Header | String | O | Token ID |


#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| routingtable | Body | Object | Port information object |
| routingtable.id | Body | UUID | Routing table ID |
| routingtable.name | Body | String | Routing table name |
| routingtable.default_table | Body | Boolean | Whether routing table is default table |
| routingtable.distributed | Body | Boolean | Routing method of routing table to query<br>`true`: Distributed, `false`: Centralized |
| routingtable.gateway_id | Body | UUID | The ID of the internet gateway when the gateway is associated with routing table |
| routingtable.tenant_id | Body | String | Tenant ID to which routing table is included |
| routingtable.state | Body | String | Status of the routing table. Currently, only the `available` status exists |
| routingtable.vpcs | Body | Array | List of VPC information objects to which routing table belongs |
| routingtable.vpcs.id | Body | UUID | VPC ID to which routing table is included |
| routingtable.subnets | Body | Array | List of subnet information objects associated with routing table | 
| routingtable.subnets.id | Body | UUID | Subnet ID linked to routing table | 
| routingtable.routes | Body | Array | List of route information objects set to routing table |
| routingtable.routes.id | Body | UUID | Route ID |
| routingtable.routes.cidr | Body | String | Route destination CIDR |
| routingtable.routes.mask | Body | String | Netmask of route destination CIDR |
| routingtable.routes.gateway | Body | String | Route gateway IP |
| routingtable.routes.gateway_id | Body | String | For routes to an internet gateway, the internet gateway ID |
| routingtable.routes.routingtable_id | Body | String | Table ID to which route is included |
| routingtable.routes.tenant_id | Body | String | Tenant ID that route belongs to |
| routingtable.create_time | Date | Routing table creation time |

<details><summary>Example</summary>
<p>

```json
{
  "routingtable": {
    "subnets": [
      "cc39a1a1-4539-46b5-91a9-80c0eacb8184",
      "ccebb749-fb04-4f52-b18d-141326c0c9c5"
    ],
    "name": "3rd-router2",
    "vpcs": [
      "86325d48-fbb1-4117-8fa8-bcbf8c692c7f"
    ],
    "tenant_id": "8189ec9dc39c4a418359603e2b84a754",
    "activated": true,
    "distributed": true,
    "state": "available",
    "default_table": true,
    "create_time": "2021-09-02 08:47:02",
    "routes": [
      {
        "tenant_id": "8189ec9dc39c4a418359603e2b84a754",
        "mask": 16,
        "id": "a253f579-d0f8-472f-a834-5c4133481086",
        "gateway": "local",
        "routingtable_id": "0cd5d6a2-fa9e-45c7-86e7-de72454ae08c",
        "cidr": "10.3.0.0/16",
        "hidden": false
      },
      {
        "tenant_id": "8189ec9dc39c4a418359603e2b84a754",
        "mask": 24,
        "id": "fc643476-efa1-4f0a-8fae-0c7e271ea0b5",
        "gateway": "10.3.1.12",
        "routingtable_id": "0cd5d6a2-fa9e-45c7-86e7-de72454ae08c",
        "cidr": "172.16.0.0/24",
        "hidden": false
      }
    ],
    "id": "0cd5d6a2-fa9e-45c7-86e7-de72454ae08c"
  }
}
```

</p>
</details>


### Delete Routing Table

Deletes the routing table. Route tables designated as the default routing table cannot be deleted, they are deleted when the VPC is deleted.

```
DELETE /v2.0/routingtables/{routingtableId}
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| routingtableId | URL | UUID | O | Routing table to delete |
| tokenId | Header | String | O | Token ID |


#### Response

This API does not return a response body.

## Route
### View Route List

Returns the route list set up in the routing table.

```
GET /v2.0/routes
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | Token ID |
| id | Query | UUID | - | Route ID |
| cidr | Query | String | - | Route destination CIDR |
| mask | Query | Integer | -  | Netmask of route destination CIDR (0-32) |
| gateway | Query | String | - | Route gateway IP |
| routingtable_id |  Query | String | - | Table ID to which route is set up|
| gateway_id |  Query | String | - | Internet gateway ID |

#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| routes | Body | Array | List of route information objects |
| routes.id | Body | UUID | Route ID |
| routes.cidr | Body | String | Route destination CIDR | 
| routes.mask | Body | Integer | Netmask of route destination CIDR |
| routes.gateway | Body | String | Route Gateway IP. "local" for a local route |
| routes.gateway_id | Body | UUID | The ID of the internet gateway, if the route was created automatically by attaching the internet gateway to the route table. |
| routes.tenant_id | Body | String | Tenant ID that route belongs to |

<details><summary>Example</summary>
<p>

```json
{
  "routes": [
    {
      "tenant_id": "8189ec9dc39c4a418359603e2b84a754",
      "mask": 16,
      "gateway": "local",
      "routingtable_id": "0101dbfa-d504-4a1f-ae28-45d721e8cf69",
      "cidr": "172.16.0.0/16",
      "id": "02ab0653-42d4-433f-81d9-776ceb23f3bf"
    },
    {
      "tenant_id": "8189ec9dc39c4a418359603e2b84a754",
      "mask": 24,
      "gateway": "172.16.10.19",
      "routingtable_id": "0101dbfa-d504-4a1f-ae28-45d721e8cf69",
      "cidr": "192.168.0.0/24",
      "id": "83729f84-90c2-422f-8f08-394e1e4310bb"
    },
    {
      "gateway_id": "ad7f3e7f-35b9-4ff1-b7b9-2451d7fc9982",
      "tenant_id": "8189ec9dc39c4a418359603e2b84a754",
      "mask": 0,
      "gateway": "100.127.64.9",
      "routingtable_id": "0101dbfa-d504-4a1f-ae28-45d721e8cf69",
      "cidr": "0.0.0.0/0",
      "id": "a9e4a335-a251-4387-9fef-f98c58281ce7"
    }
  ]
}
```

</p>
</details>

### View Route

Views the specified route.

```
GET /v2.0/routes/{routeId}
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| routeId | URL | UUID | O | Route ID to query |
| tokenId | Header | String | O | Token ID |

#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| route | Body | Array | Route information object |
| route.id | Body | UUID | Route ID |
| route.cidr | Body | String | Route destination CIDR | 
| route.mask | Body | Integer | Netmask of route destination CIDR |
| route.gateway | Body | String | Route Gateway IP. "local" for a local route |
| route.gateway_id | Body | UUID | The ID of the internet gateway, if the route was created automatically by attaching the internet gateway to the route table. |
| route.tenant_id | Body | String | Tenant ID that route belongs to |

<details><summary>Example</summary>
<p>

```json
{
  "route": {
    "tenant_id": "8189ec9dc39c4a418359603e2b84a754",
    "mask": 24,
    "gateway": "172.16.10.19",
    "routingtable_id": "0101dbfa-d504-4a1f-ae28-45d721e8cf69",
    "cidr": "192.168.0.0/24",
    "id": "83729f84-90c2-422f-8f08-394e1e4310bb"
  }
}
```

</p>
</details>


### Create Route

Add a new route to the routing table.

```
POST /v2.0/routes
X-Auth-Token: {tokenId}
```

#### Request

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | Token ID |
| routingtable_id | Body | UUID | O | Routing table ID to add a route |
| cidr | Body | String | O | Route destination CIDR |
| gateway| Body | String | O | Route gateway IP |

<details><summary>Example</summary>
<p>

```json
{
  "route": {
    "routingtable_id": "0101dbfa-d504-4a1f-ae28-45d721e8cf69", 
    "cidr": "192.168.0.0/24", 
    "gateway": "172.16.10.19"
  }
}
```

</p>
</details>

#### Response

| Name | Type | Format | Description |
| --- | --- | --- | --- |
| route | Body | Array | Route information object |
| route.id | Body | UUID | Route ID |
| route.cidr | Body | String | Route destination CIDR | 
| route.mask | Body | Integer | Netmask of route destination CIDR |
| route.gateway | Body | String | Route gateway IP |
| route.tenant_id | Body | String | Tenant ID that route belongs to |

<details><summary>Example</summary>
<p>

```json
{
  "route": {
    "tenant_id": "8189ec9dc39c4a418359603e2b84a754",
    "mask": 24,
    "gateway": "172.16.10.19",
    "routingtable_id": "0101dbfa-d504-4a1f-ae28-45d721e8cf69",
    "cidr": "192.168.0.0/24",
    "id": "83729f84-90c2-422f-8f08-394e1e4310bb"
  }
}
```

</p>
</details>

### Delete Route

Delete the specified route. Routes with a `gateway` entry of "local" or automatically added due to an Internet gateway connection (with a `gateway_id` value) cannot be deleted.

```
DELETE /v2.0/routes/{routeId}
X-Auth-Token: {tokenId}
```

#### Request

This API does not require a request body.

| Name | Type | Format | Required | Description |
| --- | --- | --- | --- | --- |
| routeId | URL | UUID | O | Route ID to delete |
| tokenId | Header | String | O | Token ID |


#### Response

This API does not return a response body.


---