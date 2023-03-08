## Network > VPC > API Guide

API is currently available only in the Korea region.

## Prerequisites

Using a network VPC API requires an appkey and a token. Get an appkey and a token by using [API Endpoint URL](/Compute/Instance/en/api-guide/#api-endpoint-url) and [Token API](/Compute/Instance/en/api-guide/#api): include the appkey to API Endpoint URL and the token to the Request Body.

For example, List Security Groups must be requested to the following URL:  

	GET https://api-compute.nhncloudservice.com/compute/v1.0/appkeys/{appkey}/security-groups?id={securityGroupId}


## Security Group API
Create, delete, list and update security groups are available. Register/unregister security groups to instances is provided by [Instance API](/Compute/Instance/en/api-guide/).

### List Security Groups
List information of accessible security groups.

#### Method, URL
```
GET /v1.0/appkeys/{appkey}/security-groups?id={securityGroupId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | Token ID |
| securityGroupId | Query | String | O | Security Group ID to list: if left empty, list information of all security groups. |

#### Request Body
This API does not require a request body.

#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    },
    "securityGroups": [
        {
            "description": "{Desctiption}",
            "id": "{Security Group ID}",
            "name": "{Name}",
            "securityGroupRules": [
                {
                    "direction": "egress",
                    "ethertype": "IPv4",
                    "id": "3c0e45ff-adaf-4124-b083-bf390e5482ff",
                    "portRangeMax": null,
                    "portRangeMin": null,
                    "protocol": null,
                    "remoteGroupId": null,
                    "remoteIpPrefix": null,
                    "securityGroupId": "85cc3048-abc3-43cc-89b3-377341426ac5",
                    "description": ""
                }
            ]
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Description | Body | String | Description of a security group |
| Security Group ID | Body | String | Security group ID |
| Name | Body | String | Name of a security group |
| securityGroupRules | Body | List | List of security group rules, in reference of [Security Group Rules API](#api_1) |

### Create Security Groups
Create a new security group.

#### Method, URL
```
POST /v1.0/appkeys/{appkey}/security-groups
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | Token ID |

#### Request Body
```json
{
    "securityGroup": {
        "name": "{Name}",
        "description": "{Description}"
    }
}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| Name | Body | String | - |Name of a security group |
| Description | Body | String | O | Description of a security group |

#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    },
    "securityGroup": {
        "description": "{Description}",
        "id": "{Security Group ID}",
        "name": "{Name}",
        "securityGroupRules": [
            {
                "direction": "egress",
                "ethertype": "IPv4",
                "id": "3c0e45ff-adaf-4124-b083-bf390e5482ff",
                "portRangeMax": null,
                "portRangeMin": null,
                "protocol": null,
                "remoteGroupId": null,
                "remoteIpPrefix": null,
                "securityGroupId": "85cc3048-abc3-43cc-89b3-377341426ac5",
                "description": ""
            }
        ]
    }
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Description | Body | String | Description of a security group |
| Security Group ID | Body | String | Security group ID |
| Name | Body | String | Name of a security group |
| securityGroupRules | Body | List | List of security group rules, in reference of [Security Group Rules API](#api_1) |

### Modify Security Groups
Modify name and description of a security group.

#### Method, URL
```
PUT /v1.0/appkeys/{appkey}/security-groups/{securityGroupId}
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | Token ID |
| securityGroupId | Path | String | - | Security group ID to modify |

#### Request Body
```json
{
    "securityGroup": {
        "name": "{Name}",
        "description": "{Description}"
    }
}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| Name | Body | String | - | Name of a security group |
| Description | Body | String | O | Description of a security group |

#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    },
    "securityGroup": {
        "id": "{Security Group ID}",
        "name": "{Name}",
        "description": "{Description}"
    }
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Security Group ID | Body | String | Security group ID |
| Name | Body | String | Name of a security group |
| Description | Body | String | Description of a security group |

### Delete Security Groups
Delete a specified security group.

#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/security-groups?id={securityGroupId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | Token ID |
| securityGroupId | Query | String | - | Security group ID to delete |

#### Request Body
This API does not require a request body.

#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    }
}
```


## Security Group Rules API
Add/Delete and List security group rules.  

### List Security Group Rules
List information of all accessible security group rules.  
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/security-group-rules?id={securityGroupRuleId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | Token ID |
| securityGroupRuleId | Query | String | O | ID of a security group rule to retrieve: if left empty, retrieve information of all security group rules. |

#### Request Body
This API does not require a request body.

#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    },
    "securityGroupRules": [
        {
            "direction": "{Direction}",
            "ethertype": "{Ethernet Type}",
            "id": "{Rule ID}",
            "portRangeMax": "{Port Range MAX}",
            "portRangeMin": "{Port Range MIN}",
            "protocol": "{Protocol}",
            "remoteGroupId": "{Remote Group ID}",
            "remoteIpPrefix": "{Remote IP Prefix}",
            "securityGroupId": "{Security Group ID}"
        }
    ]
}
```

|  Name | In | Type | Description |
| --- | --- | --- | --- |
| Direction | Body | String | Direction where rules are applied: "ingress" or "egress" |
| Ethernet Type | Body | String | "IPv4" or "IPv6" |
| Rule ID | Body | String | Security group rules ID |
| Port Range MAX | Body | Integer | Maximum port number where rules are applied |
| Port Range MIN | Body | Integer | Minimum port number where rules are applied |
| Protocol | Body | String | IP protocol: "icmp", "tcp", "udp", or "null" |
| Remote Group ID | Body | String | ID of a remote group where rules are applied |
| Remote IP Prefix | Body | String | Prefix of a remote IP where rules are applied |
| Security Group ID | Body | String | ID of a security group where rules are applied |

### Create Security Group Rules
Create a new security group rule.
#### Method, URL
```
POST /v1.0/appkeys/{appkey}/security-group-rules
X-Auth-Token: {tokenId}
Content-Type: application/json;charset=UTF-8
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | Token ID |

#### Request Body
```json
{
    "securityGroupRule": {
        "direction": "{Direction}",
        "ethertype": "{Ethernet Type}",
        "portRangeMin": "{Port Range MAX}",
        "portRangeMax": "{Port Range MIN}",
        "protocol": "{Protocol}",
        "remoteGroupId": "{Remote Group ID}",
        "remoteIpPrefix": "{Remote IP Prefix}",
        "securityGroupId": "{Security Group ID}"
    }
}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| Direction | Body | String | - | Direction where rules are applied: "ingress" or "egress" |
| Ethernet Type | Body | String | - | "IPv4" or "IPv6" |
| Port Range MAX | Body | Integer | O | Maximum port number where rules are applied, between 1 and 65535: with this setting, cannot omit "protocol" |
| Port Range MIN | Body | Integer | O | Minimum port number where rules are applied, between 1 and 65535: with this setting, cannot omit "protocol" |
| Protocol | Body | String | O | IP protocol: "icmp", "tcp", "udp", or "null" |
| Remote Group ID | Body | String | O | ID of a remote security group where rules are applied <br />: to be omitted if "remoteIpPrefix" is set <Paste> |
| Remote IP Prefix | Body | String | O | Prefix of a remote IP where rules are applied <br />: to be omitted if "remoteGroupId" is set. |
| Security Group ID | Body | String | - | ID of a security group where rules are applied |

> [Notice]
> If both `Remote Group ID` and` Remote IP Prefix` is omitted, the default value 0.0.0.0/0 is set to.
> In other words, a rule that applies to all traffic is created.

##### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    },
    "securityGroupRule": {
        "direction": "{Direction}",
        "ethertype": "{Ethernet Type}",
        "id": "{Rule ID}",
        "portRangeMax": "{Port Range MAX}",
        "portRangeMin": "{Port Range MIN}",
        "protocol": "{Protocol}",
        "remoteGroupId": "{Remote Group ID}",
        "remoteIpPrefix": "{Remote IP Prefix}",
        "securityGroupId": "{Security Group ID}"
    }
}
```

|  Name | In | Type | Description |
| --- | --- | --- | --- |
| Direction | Body | String | Direction where rules are applied: "ingress" or "egress" |
| Ethernet Type | Body | String | "IPv4" or "IPv6" |
| Rule ID | Body | String | ID of security group rules |
| Port Range MAX | Body | Integer | Maximum port number where rules are applied |
| Port Range MIN | Body | Integer | Minimum port number where rules are applied |
| Protocol | Body | String | IP protocol: "icmp", "tcp", "udp", or "null" |
| Remote Group ID | Body | String | ID of a remote security group where rules are applied |
| Remote IP Prefix | Body | String | Prefix of a remote IP where rules are applied |
| Security Group ID | Body | String | ID of a security group where rules are applied |

### Delete Security Group Rules
Delete specified security group rules.
#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/security-group-rules?id={securityGroupRuleId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | - | Token ID |
| securityGroupRuleId | Query | String | - | ID of a security group rule to delete |

#### Request Body
This API does not require a request body.

#### Response Body
```json
{
    "header" : {
        "isSuccessful" :  true,
        "resultCode" :  0,
        "resultMessage" :  "SUCCESS"
    }
}
```

## Network API
Get network information which can be accessed from an instance.

### Network Status
Networks have the following status values:

| Status | Description |
| -- | -- |
| BUILD | Network is building |
| ACTIVE | Network is active |
| DOWN | Network is inactive |
| ERROR | Error has occurred |

### Get Network Information
Get information of an accessible network.

#### Method, URL
```
GET /v1.0/appkeys/{appkey}/networks?id={networkId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional |Description |
| -- | -- | -- | -- | -- |
| tokenId | Header | String | - | Token ID |
| networkId | Query | String | O | Network ID to retrieve: if left empty, retrieve information of all networks. |

#### Request Body
This API does not require a request body.

#### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "networks": [
        {
            "adminStateUp": "{Administrative State}",
            "id": "{Network ID}",
            "name": "{Network Name}",
            "router:external": "{External Router Provided}",
            "status": "{Network Status}",
            "subnets": [
                "{Subnet ID}"
            ]
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Administrative State | Body | Boolean |Network management status: true: up, or false: down |
| Network ID | Body | String | Network ID |
| Network Name | Body | String | Network name |
| External Router Provided | Body | Boolean | Availability of a floating IP provided via router |
| Network Status | Body | String | Network status: ACTIVE, DOWN, BUILD, or ERROR |
| Subnet ID | Body | String | Subnet ID |

## Subnet API
### Get Subnet Information
Get information of an accessible subnet.
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/subnets
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional |Description |
| -- | -- | -- | -- | -- |
| tokenId | Header | String | - | Token ID |

#### Request Body
This API does not require a request body.

#### Response Body
```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "subnets": [
        {
            "allocationPools": [
                {
                    "start": "{Start IP}",
                    "end": "{End IP}"
                }
            ],
            "cidr": "{CIDR}",
            "enableDhcp": "{Enable DHCP}",
            "gatewayIp": "{Gateway IP}",
            "hostRoutes": [],
            "id": "{Subnet ID}",
            "ipVersion": "{IP version}",
            "name": "{Subnet Name}",
            "networkId": "{Network ID}"
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Start IP | Body | String | Starting IP of an allocated pool e.g) 10.161.244.13 |
| End IP | Body | String | Last IP of an allocated pool e.g) 10.161.244.121 |
| CIDR | Body | String | Refers to Classless Inter-Domain Routing, one of the ways to allocate IP addresses  e.g.) 10.161.244.0/25 |
| Enable DHCP | Body | Boolean | Whether DHCP is enable |
| Subnet ID | Body | String | Subnet ID |
| IP Version | Body | Integer | IP version of a subnet |
| Subnet Name | Body | Integer | Subnet name |
| Network ID | Body | Integer | Network ID where a subnet belongs to |

## Floating IP API
Create, Delete, and Retrieve Floating IPs.

### Floating IP Status
Floating IPs have the following status values:

| Status | Description |
| -- | -- |
| ACTIVE | Floating IP is associated to an instance and now in use |
| DOWN | Floating IP is not associated |
| ERROR | Error has occurred |


### List Pool of Floating IPs
List the pool of floating IPs.

#### Method, URL
```
GET /v1.0/appkeys/{appkey}/floating-ip-pools
X-Auth-Token: {tokenId}
```
|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | Token ID |

#### Request Body
This API does not require a request body.

#### Response Body
```json
{
    "header" : {
        "resultMessage" :  "SUCCESS",
        "isSuccessful" :  true,
        "resultCode" :  0
    },
    "pools" : [
         {
              "id" :  "{Pool ID}",
              "name" :  "{Pool Name}"
         }
    ]
}
```
|  Name | In | Type | Description |
|--|--|--|--|
| Pool ID | Body | String | Pool identifier of a floating IP |
| Pool Name | Body | String | Pool name of a floating IP |


### Get Floating IP
Get information of an available or in-use floating IP.
#### Method, URL
```
GET /v1.0/appkeys/{appkey}/floating-ips?id={floatingIpId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - |Token ID |
| floatingIpId | Query | String | O | ID of a floating IP to retrieve: if left empty, retrieve information of all floating IPs. |

#### Request Body
This API does not require a request body.

#### Response Body
```json
{
	"header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "floatingips": [
        {
        	"id": "{Floating IP ID}",
            "floatingIpAddress": "{Floating IP Address}",
            "fixedIpAddress": "{Fixed IP Address}",
            "portId": "{Port ID}",
            "routerId": "{Router ID}",
            "pool" : {
                "id" :  "{Pool ID}",
                "name" :  "{Pool Name}"
            },
            "status": "{Status}"
        }
    ]
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Floating IP ID | Body | String | ID of a floating IP |
| Floating IP Address | Body | String | Address of a floating IP |
| Fixed IP Address | Body | String | IP address of instance NIC to which floating IP is associated. Displays only when the status is "ACTIVE". |
| Port ID | Body | String | Port ID to which a floating IP is associated. Displays only when the status is "ACTIVE". |
| Router ID | Body | String | Router ID of a floating IP. Displays only when the status is "ACTIVE". |
| Pool ID | Body | String | Pool identifier where a floating IP belongs to |
| Pool Name | Body | String | Name of a pool where a floating IP belongs to |
| Status | Body | String | Status of a floating IP |

### Create Floating IPs
Create a floating IP.
#### Method, URL
```
POST /v1.0/appkeys/{appkey}/floating-ips
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | Token ID |

#### Request Body
```json
{
        "pool" : {
                "id" :  "{Pool ID}"
        }
}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
|  Pool ID | Body | String | - | Pool identifier of a floating IP |

#### Response Body
```json
{
	"header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    },
    "floatingip": {
    	"id": "{Floating IP ID}",
        "floatingIpAddress": "{Floating IP Address}",
        "pool": {
              "id" :  "{Pool ID}",
              "name" :  "{Pool Name}"
        },
        "status": "{Status}"
    }
}
```

|  Name | In | Type | Description |
|--|--|--|--|
| Floating IP ID | Body | String | ID of a floating IP |
| Floating IP Address | Body | String | Address of a floating IP |
| Pool ID | Body | String | Pool identifier to where a floating IP belongs |
| Pool Name | Body | String | Name of a pool to where a floating IP belongs |
| Status | Body | String | Status of a floating IP |

### Delete Floating IP
Delete a designated floating IP. For an active floating IP, disassociate first and delete.  
#### Method, URL
```
DELETE /v1.0/appkeys/{appkey}/floating-ips?id={floatingIpId}
X-Auth-Token: {tokenId}
```

|  Name | In | Type | Optional | Description |
|--|--|--|--|--|
| tokenId | Header | String | - | Token ID |
| floatingIpId | Path | String | - | ID of a floating IP to delete |

#### Request Body
This API does not require a  request body.

#### Response Body

```json
{
    "header": {
        "isSuccessful": true,
        "resultCode": 0,
        "resultMessage": "SUCCESS"
    }
}
```
