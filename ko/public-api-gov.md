## Network > VPC > API v2 가이드

API를 사용하려면 API 엔드포인트와 토큰 등이 필요합니다. [API 사용 준비](/Compute/Compute/ko/identity-api-gov/)를 참고하여 API 사용에 필요한 정보를 준비합니다.

VPC API는 `network` 타입 엔드포인트를 이용합니다. 정확한 엔드포인트는 토큰 발급 응답의 `serviceCatalog`를 참조합니다.

| 타입 | 리전 | 엔드포인트 |
|---|---|---|
| network | 한국(판교) 리전 | https://kr1-api-network-infrastructure.gov-nhncloudservice.com |

API 응답에 가이드에 명시되지 않은 필드가 나타날 수 있습니다. 이런 필드는 NHN Cloud 내부 용도로 사용되며 사전 공지 없이 변경될 수 있으므로 사용하지 않습니다.

## VPC
### VPC 목록 보기
사용 가능한 VPC 목록을 반환합니다.
```
GET /v2.0/vpcs
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.


| 이름              | 종류     | 형식      | 필수  | 설명                                                                        |
|-----------------|--------|---------|-----|---------------------------------------------------------------------------|
| tokenId         | Header | String  | O   | 토큰 ID                                                                     |
| tenant_id       | Query  | String  | -   | 조회할 네트워크가 속한 테넌트 ID                                                       |
| id              | Query  | UUID    | -   | 조회할 네트워크 ID                                                               |
| name            | Query  | String  | -   | 조회할 네트워크 이름                                                               |
| router:external | Query  | Boolean | -   | 조회할 네트워크의 외부 연결 여부                                                        |
| shared          | Query  | Boolean | -   | 조회할 네트워크의 공유 여부                                                           |
| tenant_id       | Query  | String  | -   | 조회할 네트워크가 속한 테넌트 ID                                                       |
| sort_dir        | Query  | Enum    | -   | 조회할 네트워크의 정렬 방향<br>`sort_key`에서 지정한 필드를 기준으로 정렬<br>**asc**, **desc** 중 하나 |
| sort_key        | Query  | String  | -   | 조회할 네트워크의 정렬 키<br>`sort_dir`에서 지정한 방향대로 정렬                                |


#### 응답
| 이름                   | 종류   | 형식      | 설명                 |
|----------------------|------|---------|--------------------|
| vpcs                 | Body | Array   | VPC 정보 객체 목록       |
| vpcs.router:external | Body | Boolean | 조회할 VPC의 외부 연결 여부  |
| vpcs.name            | Body | String  | 조회할 VPC 이름         |
| vpcs.tenant_id       | Body | UUID    | 조회할 VPC가 속한 테넌트 ID |
| vpcs.state           | Body | String  | 조회활 VPC의 상태        |
| vpcs.create_time     | Body | Date    | 조회할 VPC의 생성 시간     |
| vpcs.cidrv4          | Body | String  | 조회할 VPC의 IP 대역     |
| vpcs.shared          | Body | Boolean | 조회할 VPC의 공유 여부     |
| vpcs.id              | Body | UUID    | 조회할 VPC의 ID        |

<details><summary>예시</summary>
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

### VPC 보기
지정한 VPC를 조회합니다.
```
GET /v2.0/vpcs/{vpcId}  
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름        | 종류     | 형식     | 필수  | 설명                  |
|-----------|--------|--------|-----|---------------------|
| vpcId     | URL    | String | O   | VPC ID              |
| tokenId   | Header | String | O   | 토큰 ID               |

#### 응답

| 이름                                     | 종류   | 형식       | 설명                                |
|----------------------------------------|------|----------|-----------------------------------|
| vpc                                    | Body | Array    | VPC 정보 객체                         |
| vpc.router:external                    | Body | Boolean  | 조회할 VPC의 외부 연결 여부                 |
| vpc.routingtables                      | Body | Array    | 조회할 VPC의                          |
| vpc.name                               | Body | String   | 조회할 VPC 이름                        |
| vpc.subnets                            | Body | Array    | 조회할 VPC의 서브넷 목록                   | 
| vpc.subnets.router:external            | Body | Boolean  | 서브넷의 외부 연결 여부                     |
| vpc.subnets.name                       | Body | String   | 서브넷의 이름                           |
| vpc.subnets.enable_dhcp                | Body | Boolean  | 서브넷의 DHCP 활성화 여부                  |
| vpc.subnets.tenant_id                  | Body | UUID     | 서브넷의 테넨트 ID                       |
| vpc.subnets.gateway                    | Body | String   | 서브넷의 게이트웨이 IP 정보                  |
| vpc.subnets.routingtable               | Body | Object   | 서브넷의 라우팅 테이블 정보                   |
| vpc.subnets.routingtable.gateway_id    | Body | UUID     | 라우팅 테이블의 게이트 웨이 ID                |
| vpc.subnets.routingtable.default_table | Body | Boolean  | 라우팅 테이블의 Default Routing Table 여부 |
| vpc.subnets.routingtable.explicit      | Body | Boolean  | 라우팅 테이블의 명시적 연결 여부                |
| vpc.subnets.routingtable.id            | Body | UUID     | 라우팅 테이블의 ID                       |
| vpc.subnets.routingtable.name          | Body | String   | 라우팅 테이블의 이름                       |
| vpc.subnets.routes                     | Body | Array    | 서브넷의 추가 경로                        |
| vpc.subnets.routes.subnet_id           | Body | UUID     | 추가 경로의 서브넷 ID                     |
| vpc.subnets.routes.tenant_id           | Body | UUID     | 추가 경로의 테넨트 ID                     |
| vpc.subnets.routes.mask                | Body | Integer  | 추가 경로의 마스크                        |
| vpc.subnets.routes.gateway             | Body | String   | 추가 경로의 게이트 웨이 IP                  |
| vpc.subnets.routes.cidr                | Body | String   | 추가 경로의 IP 대역                      |
| vpc.subnets.routes.id                  | Body | UUID     | 추가 경로의  ID                        |
| vpc.subnets.state                      | Body | String   | 서브넷의 상태                           |
| vpc.subnets.create_time                | Body | Date     | 서브넷의 생성 시각                        |
| vpc.subnets.available_ip_count         | Body | Interger | 서브넷의 할당 가능한 IP 개수                 | 
| vpc.subnets.vpc                        | Body | Object   | 서브넷의 VPC 정보                       |
| vpc.subnets.vpc.shared                 | Body | Boolean  | VPC의 공유 여부                        |
| vpc.subnets.vpc.state                  | Body | String   | VPC의 상태                           |
| vpc.subnets.vpc.id                     | Body | UUID     | VPC의 ID                           |
| vpc.subnets.vpc.cidrv4                 | Body | String   | 조회할 VPC의 IP 대역                    |
| vpc.subnets.vpc.name                   | Body | String   | VPC의 이름                           |
| vpc.subnets.shared                     | Body | Boolean  | 서브넷의 공유 여부                        |
| vpc.subnets.id                         | Body | UUID     | 서브넷의 ID                           |
| vpc.subnets.vpc_id                     | Body | UUID     | 서브넷의 VPC ID                       |
| vpc.subnets.cidr                       | Body | String   | 서브넷의 CIDR IP                      |
| vpc.tenant_id                          | Body | UUID     | 조회할 VPC가 속한 테넌트 ID                |
| vpc.state                              | Body | String   | 조회활 VPC의 상태                       |
| vpc.create_time                        | Body | Date     | 조회할 VPC의 생성 시간                    |
| vpc.cidrv4                             | Body | String   | 조회할 VPC의 IPv4                     |
| vpc.shared                             | Body | Boolean  | 조회할 VPC의 공유 여부                    |
| vpc.id                                 | Body | UUID     | 조회할 VPC의 ID                       |

<details><summary>예시</summary>
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

### VPC 생성하기

```
POST /v2.0/vpcs 
X-Auth-Token: {tokenId}
```

#### 요청
| 이름                      | 종류     | 형식     | 필수  | 설명                                   |
|-------------------------|--------|--------|-----|--------------------------------------|
| tokenId                 | Header | String | O   | 토큰 ID                                |
| vpc                     | Body   | Object | O   | VPC 생성 요청 객체                         |
| vpc.name                | Body   | String | O   | VPC 이름                               |
| vpc.cidrv4              | Body   | String | O   | VPC IP 대역                            |
| vpc.tenant_id           | Body   | UUID   |     | VPC의 tenant ID                       |
| vpc.external_network_id | Body   | UUID   |     | VPC에 연결될 External Network ID         |
| vpc.subnets             | Body   | Array  |     | VPC에 연결될 External Network의 서브넷 ID 배열  |
<details><summary>예시</summary>
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

#### 응답
| 이름              | 종류   | 형식      | 설명             |
|-----------------|------|---------|----------------|
| vpc             | Body | Array   | VPC 정보 객체      |
| vpc.name        | Body | String  | VPC 이름         |
| vpc.tenant_id   | Body | UUID    | VPC가 속한 테넌트 ID |
| vpc.state       | Body | String  | VPC의 상태        |
| vpc.create_time | Body | Date    | VPC의 생성 시간     |
| vpc.cidrv4      | Body | String  | VPC의 IP 대역     |
| vpc.shared      | Body | Boolean | VPC의 공유 여부     |
| vpc.id          | Body | UUID    | VPC의 ID        |
<details><summary>예시</summary>
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

### VPC 수정하기
VPC의 이름과 VPC IP 대역을 변경합니다.
```
PUT /v2.0/vpcs/{vpcId}
X-Auth-Token: {tokenId}
```
#### 요청



| 이름         | 종류     | 형식     | 필수  | 설명           |
|------------|--------|--------|-----|--------------|
| vpcId      | URL    | String | O   | VPC ID       |
| tokenId    | Header | String | O   | 토큰 ID        |
| vpc        | Body   | Object | O   | VPC 생성 요청 객체 |
| vpc.name   | Body   | String |     | VPC 이름       |
| vpc.cidrv4 | Body   | String |     | VPC IP 대역    |

<details><summary>예시</summary>
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

#### 응답
| 이름              | 종류   | 형식      | 설명             |
|-----------------|------|---------|----------------|
| vpc             | Body | Array   | VPC 정보 객체      |
| vpc.name        | Body | String  | VPC 이름         |
| vpc.tenant_id   | Body | UUID    | VPC가 속한 테넌트 ID |
| vpc.state       | Body | String  | VPC의 상태        |
| vpc.create_time | Body | Date    | VPC의 생성 시간     |
| vpc.cidrv4      | Body | String  | VPC의 IP 대역     |
| vpc.shared      | Body | Boolean | VPC의 공유 여부     |
| vpc.id          | Body | UUID    | VPC의 ID        |

<details><summary>예시</summary>
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

### VPC 삭제하기
지정한 VPC를 삭제합니다.
```
DELETE /v2.0/vpcs/{vpcId}
X-Auth-Token: {tokenId}
```
#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름      | 종류     | 형식     | 필수  | 설명     |
|---------|--------|--------|-----|--------|
| vpcId   | URL    | String | O   | VPC ID |
| tokenId | Header | String | O   | 토큰 ID  |

#### 응답
이 API는 응답 본문을 반환하지 않습니다.

## VPC 서브넷
### VPC 서브넷 목록 보기
사용 가능한 서브넷 목록을 반환합니다.

```
GET /v2.0/vpcsubnets
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| id | Query | UUID | - | 조회할 서브넷 ID |
| name | Query | String | - | 조회할 서브넷 이름 |
| vpc_id | Query | UUID | - | 조회할 서브넷의 VPC ID |
| cidr | Query | String | - | 조회할 서브넷 CIDR |
| shared | Query | Boolean | - | 조회할 서브넷의 공유 여부 |
| sort_dir | Query | Enum | - | 조회할 서브넷의 정렬 방향<br>`sort_key`에서 지정한 필드를 기준으로 정렬<br>**asc**, **desc** 중 하나 |
| sort_key | Query | String | - | 조회할 서브넷의 정렬 키<br>`sort_dir`에서 지정한 방향대로 정렬 |


#### 응답

| 이름                                    | 종류   | 형식      | 설명                        |
|---------------------------------------|------|---------|---------------------------|
| vpcsubnets                            | Body | Array   | 서브넷 정보 객체                 |
| vpcsubnets.router:external            | Body | Boolean | 서브넷의 외부 연결 여부             |
| vpcsubnets.name                       | Body | String  | 서브넷의 이름                   |
| vpcsubnets.tenant_id                  | Body | UUID    | 서브넷이 속한 테넌트 ID            |
| vpcsubnets.id                         | Body | UUID    | 서브넷의 ID                   |
| vpcsubnets.routingtable               | Body | Object  | 서브넷이 연결된 라우팅 테이블              |
| vpcsubnets.routingtable.gateway_id    | Body | UUID    | 라우팅 테이블의 게이트웨이 ID        |
| vpcsubnets.routingtable.default_table | Body | Boolean | 라우팅 테이블의 Default Table 여부 |
| vpcsubnets.routingtable.explicit      | Body | Boolean | 라우팅 테이블과의 명시적 연결 여부        |
| vpcsubnets.routingtable.id            | Body | UUID    | 라우팅 테이블의 ID               |
| vpcsubnets.routingtable.name          | Body | String  | 라우팅 테이블의 이름               |
| vpcsubnets.state                      | Body | String  | 서브넷의 상태                   |
| vpcsubnets.create_time                | Body | Date    | 서브넷의 생성 시간                |
| vpcsubnets.available_ip_count         | Body | Integer | 서브넷의 사용 가능한 IP 개수         |
| vpcsubnets.vpc                        | Body | Object  | 서브넷이 속한 VPC               |
| vpcsubnets.vpc.shared                 | Body | Boolean | VPC의 공유 여부                |
| vpcsubnets.vpc.state                  | Body | String  | VPC의 상태                   |
| vpcsubnets.vpc.id                     | Body | UUID    | VPC의 ID                   |
| vpcsubnets.vpc.cidrv4                 | Body | String  | VPC의 IP 대역                |
| vpcsubnets.vpc.name                   | Body | String  | VPC 이름                    |
| vpcsubnets.vpc_id                     | Body | UUID    | 서브넷이 속한 VPC의 ID           |
| vpcsubnets.routes                     | Body | Array   | 서브넷의 정적 경로                |
| vpcsubnets.routes.subnet_id           | Body | UUID    | 정적 경로의 서브넷 ID             |
| vpcsubnets.routes.tenant_id           | Body | UUID    | 정적 경로의 테넨트 ID             |
| vpcsubnets.routes.mask                | Body | Integer | 정적 경로의 마스크                |
| vpcsubnets.routes.gateway             | Body | String  | 정적 경로의 게이트웨이 IP           |
| vpcsubnets.routes.cidr                | Body | String  | 정적 경로의 대상 IP 대역              |
| vpcsubnets.routes.id                  | Body | UUID    | 추가 경로의 ID                 |
| vpcsubnets.shared                     | Body | Boolean | 서브넷의 공유 여부                |
| vpcsubnets.cidr                       | Body | String  | 서브넷의 IP 대역                |
| vpcsubnets.gateway                    | Body | String  | 서브넷의 게이트웨이 IP         |


<details><summary>예시</summary>
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

### VPC 서브넷 보기
지정한 서브넷을 조회합니다.
```
GET /v2.0/vpcsubnets/{subnetId}  
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름       | 종류     | 형식     | 필수  | 설명     |
|----------|--------|--------|-----|--------|
| subnetId | URL    | UUID   | O   | 서브넷 ID |
| tokenId  | Header | String | O   | 토큰 ID  |


#### 응답

| 이름                                   | 종류   | 형식      | 설명                        |
|--------------------------------------|------|---------|---------------------------|
| vpcsubnet                            | Body | Array   | 서브넷 정보 객체                 |
| vpcsubnet.router:external            | Body | Boolean | 서브넷의 외부 연결 여부             |
| vpcsubnet.name                       | Body | String  | 서브넷의 이름                   |
| vpcsubnet.tenant_id                  | Body | UUID    | 서브넷이 속한 테넌트 ID            |
| vpcsubnet.id                         | Body | UUID    | 서브넷의 ID                   |
| vpcsubnet.routingtable               | Body | Object  | 서브넷이 연결된 라우팅 테이블              |
| vpcsubnet.routingtable.gateway_id    | Body | UUID    | 라우팅 테이블의 게이트웨이 ID        |
| vpcsubnet.routingtable.default_table | Body | Boolean | 라우팅 테이블의 Default Table 여부 |
| vpcsubnet.routingtable.explicit      | Body | Boolean | 라우팅 테이블과의 명시적 연결 여부        |
| vpcsubnet.routingtable.id            | Body | UUID    | 라우팅 테이블의 ID               |
| vpcsubnet.routingtable.name          | Body | String  | 라우팅 테이블의 이름               |
| vpcsubnet.state                      | Body | String  | 서브넷의 상태                   |
| vpcsubnet.create_time                | Body | Date    | 서브넷의 생성 시간                |
| vpcsubnet.available_ip_count         | Body | Integer | 서브넷의 사용 가능한 IP 개수         |
| vpcsubnet.vpc                        | Body | Object  | 서브넷이 속한 VPC               |
| vpcsubnet.vpc.shared                 | Body | Boolean | VPC의 공유 여부                |
| vpcsubnet.vpc.state                  | Body | String  | VPC의 상태                   |
| vpcsubnet.vpc.id                     | Body | UUID    | VPC의 ID                   |
| vpcsubnet.vpc.cidrv4                 | Body | String  | VPC의 IP 대역                |
| vpcsubnet.vpc.name                   | Body | String  | VPC 이름                    |
| vpcsubnet.vpc_id                     | Body | UUID    | 서브넷이 속한 VPC의 ID           |
| vpcsubnet.routes                     | Body | Array   | 서브넷의 정적 경로                |
| vpcsubnet.routes.subnet_id           | Body | UUID    | 정적 경로의 서브넷 ID             |
| vpcsubnet.routes.tenant_id           | Body | UUID    | 정적 경로의 테넨트 ID             |
| vpcsubnet.routes.mask                | Body | Integer | 정적 경로의 마스크                |
| vpcsubnet.routes.gateway             | Body | String  | 정적 경로의 게이트웨이 IP           |
| vpcsubnet.routes.cidr                | Body | String  | 정적 경로의 대상 IP 대역              |
| vpcsubnet.routes.id                  | Body | UUID    | 정적 경로의 ID                 |
| vpcsubnet.shared                     | Body | Boolean | 서브넷의 공유 여부                |
| vpcsubnet.cidr                       | Body | String  | 서브넷의 IP 대역                |
| vpcsubnet.gateway                    | Body | String  | 서브넷의 게이트웨이 IP         |


<details><summary>예시</summary>
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

### VPC 서브넷 생성하기
새로운 서브넷을 생성합니다.
```
POST /v2.0/vpcsubnets  
X-Auth-Token: {tokenId}
```
#### 요청

| 이름                  | 종류     | 형식     | 필수  | 설명              |
|---------------------|--------|--------|-----|-----------------|
| tokenId             | Header | String | O   | 토큰 ID           |
| vpcsubnet           | Body   | Object | O   | 서브넷 정보 객체       |
| vpcsubnet.vpc_id    | Body   | UUID   | O   | 서브넷이 할당될 VPC ID |
| vpcsubnet.cidr      | Body   | String | O   | 서브넷의 IP 대역      |
| vpcsubnet.name      | Body   | String | O   | 서브넷의 이름         |
| vpcsubnet.tenant_id | Body   | UUID   |     | 서브넷이 할당될 테넨트 ID |

<details><summary>예시</summary>
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

#### 응답

| 이름                    | 종류   | 형식      | 설명                |
|-----------------------|------|---------|-------------------|
| vpcsubnet             | Body | Array   | 서브넷 정보 객체         |
| vpcsubnet.name        | Body | String  | 서브넷의 이름           |
| vpcsubnet.tenant_id   | Body | String  | 서브넷이 속한 테넌트 ID    |
| vpcsubnet.id          | Body | UUID    | 서브넷의 ID           |
| vpcsubnet.state       | Body | String  | 서브넷의 상태           |
| vpcsubnet.create_time | Body | Date    | 서브넷의 생성 시간        |
| vpcsubnet.vpc_id      | Body | String  | 서브넷이 속한 VPC의 ID   |
| vpcsubnet.shared      | Body | Boolean | 서브넷의 공유 여부        |
| vpcsubnet.cidr        | Body | String  | 서브넷의 IP 대역        |
| vpcsubnet.gateway     | Body | String  | 서브넷의 게이트웨이 IP |

<details><summary>예시</summary>
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

### VPC 서브넷 수정하기
서브넷의 이름을 변경합니다.
```
PUT /v2.0/vpcsubnets/{subnetId}  
X-Auth-Token: {tokenId}
```
#### 요청
| 이름             | 종류     | 형식     | 필수  | 설명        |
|----------------|--------|--------|-----|-----------|
| subnetId       | URL    | UUID   | O   | 서브넷 ID    |
| tokenId        | Header | String | O   | 토큰 ID     |
| vpcsubnet      | Body   | Object | O   | 서브넷 정보 객체 |
| vpcsubnet.name | Body   | String | O   | 서브넷의 이름   |

<details><summary>예시</summary>
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

#### 응답

| 이름                    | 종류   | 형식      | 설명                |
|-----------------------|------|---------|-------------------|
| vpcsubnet             | Body | Array   | 서브넷 정보 객체         |
| vpcsubnet.name        | Body | String  | 서브넷의 이름           |
| vpcsubnet.tenant_id   | Body | String  | 서브넷이 속한 테넌트 ID    |
| vpcsubnet.id          | Body | UUID    | 서브넷의 ID           |
| vpcsubnet.state       | Body | String  | 서브넷의 상태           |
| vpcsubnet.create_time | Body | Date    | 서브넷의 생성 시간        |
| vpcsubnet.vpc_id      | Body | String  | 서브넷이 속한 VPC의 ID   |
| vpcsubnet.shared      | Body | Boolean | 서브넷의 공유 여부        |
| vpcsubnet.cidr        | Body | String  | 서브넷의 IP 대역        |
| vpcsubnet.gateway     | Body | String  | 서브넷의 게이트웨이 IP |

<details><summary>예시</summary>
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

### VPC 서브넷을 라우팅 테이블에 연결하기
서브넷을 특정 라우팅 테이블에 명시적으로 연결합니다.
```
PUT /v2.0/vpcsubnets/{subnetId}/attach_routingtable
X-Auth-Token: {tokenId}
```
#### 요청
| 이름       | 종류     | 형식     | 필수  | 설명     |
|----------|--------|--------|-----|--------|
| subnetId | URL    | UUID   | O   | 서브넷 ID |
| tokenId  | Header | String | O   | 토큰 ID  |
| routingtable_id | Body | UUID | O   | 연결할 라우팅 테이블 ID

<details><summary>예시</summary>
<p>

```json
{
   "routingtable_id": "c577a618-1acb-444d-a82e-aeb4c6fce01e"
}
```

</p>
</details>

#### 응답

| 이름                                   | 종류   | 형식      | 설명                        |
|--------------------------------------|------|---------|---------------------------|
| vpcsubnet                            | Body | Array   | 서브넷 정보 객체                 |
| vpcsubnet.router:external            | Body | Boolean | 서브넷의 외부 연결 여부             |
| vpcsubnet.name                       | Body | String  | 서브넷의 이름                   |
| vpcsubnet.tenant_id                  | Body | UUID    | 서브넷이 속한 테넌트 ID            |
| vpcsubnet.id                         | Body | UUID    | 서브넷의 ID                   |
| vpcsubnet.routingtable               | Body | Object  | 서브넷이 연결된 라우팅 테이블              |
| vpcsubnet.routingtable.gateway_id    | Body | UUID    | 라우팅 테이블의 게이트웨이 ID        |
| vpcsubnet.routingtable.default_table | Body | Boolean | 라우팅 테이블의 Default Table 여부 |
| vpcsubnet.routingtable.explicit      | Body | Boolean | 라우팅 테이블과의 명시적 연결 여부        |
| vpcsubnet.routingtable.id            | Body | UUID    | 라우팅 테이블의 ID               |
| vpcsubnet.routingtable.name          | Body | String  | 라우팅 테이블의 이름               |
| vpcsubnet.state                      | Body | String  | 서브넷의 상태                   |
| vpcsubnet.create_time                | Body | Date    | 서브넷의 생성 시간                |
| vpcsubnet.available_ip_count         | Body | Integer | 서브넷의 사용 가능한 IP 개수         |
| vpcsubnet.vpc                        | Body | Object  | 서브넷이 속한 VPC               |
| vpcsubnet.vpc.shared                 | Body | Boolean | VPC의 공유 여부                |
| vpcsubnet.vpc.state                  | Body | String  | VPC의 상태                   |
| vpcsubnet.vpc.id                     | Body | UUID    | VPC의 ID                   |
| vpcsubnet.vpc.cidrv4                 | Body | String  | VPC의 IP 대역                |
| vpcsubnet.vpc.name                   | Body | String  | VPC 이름                    |
| vpcsubnet.vpc_id                     | Body | UUID    | 서브넷이 속한 VPC의 ID           |
| vpcsubnet.routes                     | Body | Array   | 서브넷의 정적 경로                |
| vpcsubnet.routes.subnet_id           | Body | UUID    | 정적 경로의 서브넷 ID             |
| vpcsubnet.routes.tenant_id           | Body | UUID    | 정적 경로의 테넨트 ID             |
| vpcsubnet.routes.mask                | Body | Integer | 정적 경로의 마스크                |
| vpcsubnet.routes.gateway             | Body | String  | 정적 경로의 게이트웨이 IP           |
| vpcsubnet.routes.cidr                | Body | String  | 정적 경로의 대상 IP 대역              |
| vpcsubnet.routes.id                  | Body | UUID    | 정적 경로의 ID                 |
| vpcsubnet.shared                     | Body | Boolean | 서브넷의 공유 여부                |
| vpcsubnet.cidr                       | Body | String  | 서브넷의 IP 대역                |
| vpcsubnet.gateway                    | Body | String  | 서브넷의 게이트웨이 IP         |



<details><summary>예시</summary>
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

### VPC 서브넷과 라우팅 테이블의 연결을 해제하기
서브넷과 명시적으로 연결되어 있는 라우팅 테이블의 연결을 해제합니다.

```
PUT /v2.0/vpcsubnets/{subnetId}/detach_routingtable
X-Auth-Token: {tokenId}
```
#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름       | 종류     | 형식     | 필수  | 설명     |
|----------|--------|--------|-----|--------|
| subnetId | URL    | UUID   | O   | 서브넷 ID |
| tokenId  | Header | String | O   | 토큰 ID  |

#### 응답

| 이름                                   | 종류   | 형식      | 설명                        |
|--------------------------------------|------|---------|---------------------------|
| vpcsubnet                            | Body | Array   | 서브넷 정보 객체                 |
| vpcsubnet.router:external            | Body | Boolean | 서브넷의 외부 연결 여부             |
| vpcsubnet.name                       | Body | String  | 서브넷의 이름                   |
| vpcsubnet.enable_dhcp                | Body | Boolean | 서브넷의 DHCP 활성화 여부          |
| vpcsubnet.tenant_id                  | Body | UUID    | 서브넷이 속한 테넌트 ID            |
| vpcsubnet.routingtable               | Body | Object  | 서브넷의 라우팅 테이블              |
| vpcsubnet.routingtable.gateway_id    | Body | UUID    | 라우팅 테이블의 게이트 웨이 ID        |
| vpcsubnet.routingtable.default_table | Body | Boolean | 라우팅 테이블의 Default Table 여부 |
| vpcsubnet.routingtable.explicit      | Body | Boolean | 라우팅 테이블의 명시적 연결 여부        |
| vpcsubnet.routingtable.id            | Body | UUID    | 라우팅 테이블의 ID               |
| vpcsubnet.routingtable.name          | Body | String  | 라우팅 테이블의 이름               |
| vpcsubnet.gateway                    | Body | String  | 서브넷에 연결된 게이트웨이 IP         |
| vpcsubnet.routes                     | Body | Array   | 서브넷의 추가 경로                |
| vpcsubnet.routes.subnet_id           | Body | UUID    | 추가 경로의 서브넷 ID             |
| vpcsubnet.routes.tenant_id           | Body | UUID    | 추가 경로의 테넨트 ID             |
| vpcsubnet.routes.mask                | Body | Integer | 추가 경로의 마스크                |
| vpcsubnet.routes.gateway             | Body | String  | 추가 경로의 게이트웨이 IP           |
| vpcsubnet.routes.cidr                | Body | String  | 추가 경로의 IP 대역              |
| vpcsubnet.routes.id                  | Body | UUID    | 추가 경로의 ID                 |
| vpcsubnet.state                      | Body | String  | 서브넷의 상태                   |
| vpcsubnet.create_time                | Body | Date    | 서브넷의 생성 시간                |
| vpcsubnet.available_ip_count         | Body | Integer | 서브넷이 사용 가능한 IP 개수         |
| vpcsubnet.vpc                        | Body | Object  | 서브넷가 속한 VPC               |
| vpcsubnet.vpc.shared                 | Body | Boolean | VPC의 공유 여부                |
| vpcsubnet.vpc.state                  | Body | String  | VPC의 상태                   |
| vpcsubnet.vpc.id                     | Body | UUID    | VPC의 ID                   |
| vpcsubnet.vpc.cidrv4                 | Body | String  | VPC의 IP 대역                |
| vpcsubnet.vpc.name                   | Body | String  | VPC 이름                    |
| vpcsubnet.shared                     | Body | Boolean | 서브넷의 공유 여부                |
| vpcsubnet.id                         | Body | UUID    | 서브넷의 ID                   |
| vpcsubnet.vpc_id                     | Body | UUID    | 서브넷가 속한 VPC의 ID           |
| vpcsubnet.hidden                     | Body | Boolean | 서브넷의 콘솔 노출 여부             |
| vpcsubnet.cidr                       | Body | String  | 서브넷의 IP 대역                |


<details><summary>예시</summary>
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

### VPC 서브넷 삭제하기

지정한 서브넷을 삭제합니다.

```
DELETE /v2.0/vpcsubnets/{subnetId}  
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름       | 종류     | 형식     | 필수  | 설명     |
|----------|--------|--------|-----|--------|
| subnetId | URL    | UUID   | O   | 서브넷 ID |
| tokenId  | Header | String | O   | 토큰 ID  |

#### 응답
이 API는 응답 본문을 반환하지 않습니다.

---
