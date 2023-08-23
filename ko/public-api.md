## Network > VPC > API v2 가이드

API를 사용하려면 API 엔드포인트와 토큰 등이 필요합니다. [API 사용 준비](/Compute/Compute/ko/identity-api/)를 참고하여 API 사용에 필요한 정보를 준비합니다.

VPC API는 `network` 타입 엔드포인트를 이용합니다. 정확한 엔드포인트는 토큰 발급 응답의 `serviceCatalog`를 참조합니다.

| 타입 | 리전 | 엔드포인트 |
|---|---|---|
| network | 한국(판교) 리전<br>한국(평촌) 리전<br>일본 리전 | https://kr1-api-network-infrastructure.nhncloudservice.com<br>https://kr2-api-network-infrastructure.nhncloudservice.com<br>https://jp1-api-network-infrastructure.nhncloudservice.com |

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
| vpcs.state           | Body | String  | 조회할 VPC의 상태        |
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
| vpc.routingtables                      | Body | Array    | 조회할 VPC의 라우팅 테이블 배열               |
| vpc.name                               | Body | String   | 조회할 VPC 이름                        |
| vpc.subnets                            | Body | Array    | 조회할 VPC의 서브넷 목록                   | 
| vpc.subnets.router:external            | Body | Boolean  | 서브넷의 외부 연결 여부                     |
| vpc.subnets.name                       | Body | String   | 서브넷의 이름                           |
| vpc.subnets.enable_dhcp                | Body | Boolean  | 서브넷의 DHCP 활성화 여부                  |
| vpc.subnets.tenant_id                  | Body | UUID     | 서브넷의 테넌트 ID                       |
| vpc.subnets.gateway                    | Body | String   | 서브넷의 게이트웨이 IP 정보                  |
| vpc.subnets.routingtable               | Body | Object   | 서브넷의 라우팅 테이블 정보                   |
| vpc.subnets.routingtable.gateway_id    | Body | UUID     | 라우팅 테이블의 게이트웨이 ID                 |
| vpc.subnets.routingtable.default_table | Body | Boolean  | 라우팅 테이블의 Default Routing Table 여부 |
| vpc.subnets.routingtable.explicit      | Body | Boolean  | 라우팅 테이블의 명시적 연결 여부                |
| vpc.subnets.routingtable.id            | Body | UUID     | 라우팅 테이블의 ID                       |
| vpc.subnets.routingtable.name          | Body | String   | 라우팅 테이블의 이름                       |
| vpc.subnets.routes                     | Body | Array    | 서브넷의 정적 경로                        |
| vpc.subnets.routes.subnet_id           | Body | UUID     | 정적 경로의 서브넷 ID                     |
| vpc.subnets.routes.tenant_id           | Body | UUID     | 정적 경로의 테넌트 ID                     |
| vpc.subnets.routes.mask                | Body | Integer  | 정적 경로의 마스크                        |
| vpc.subnets.routes.gateway             | Body | String   | 정적 경로의 게이트웨이 IP                   |
| vpc.subnets.routes.cidr                | Body | String   | 정적 경로의 IP 대역                      |
| vpc.subnets.routes.id                  | Body | UUID     | 정적 경로의 ID                         |
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
| vpc.state                              | Body | String   | 조회할 VPC의 상태                       |
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
| vpcsubnets.routingtable               | Body | Object  | 서브넷이 연결된 라우팅 테이블          |
| vpcsubnets.routingtable.gateway_id    | Body | UUID    | 라우팅 테이블의 게이트웨이 ID         |
| vpcsubnets.routingtable.default_table | Body | Boolean | 라우팅 테이블의 Default Table 여부 |
| vpcsubnets.routingtable.explicit      | Body | Boolean | 라우팅 테이블과의 명시적 연결 여부       |
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
| vpcsubnets.routes.tenant_id           | Body | UUID    | 정적 경로의 테넌트 ID             |
| vpcsubnets.routes.mask                | Body | Integer | 정적 경로의 마스크                |
| vpcsubnets.routes.gateway             | Body | String  | 정적 경로의 게이트웨이 IP           |
| vpcsubnets.routes.cidr                | Body | String  | 정적 경로의 대상 IP 대역           |
| vpcsubnets.routes.id                  | Body | UUID    | 정적 경로의 ID                 |
| vpcsubnets.shared                     | Body | Boolean | 서브넷의 공유 여부                |
| vpcsubnets.cidr                       | Body | String  | 서브넷의 IP 대역                |
| vpcsubnets.gateway                    | Body | String  | 서브넷의 게이트웨이 IP             |


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
| vpcsubnet.routingtable               | Body | Object  | 서브넷이 연결된 라우팅 테이블          |
| vpcsubnet.routingtable.gateway_id    | Body | UUID    | 라우팅 테이블의 게이트웨이 ID         |
| vpcsubnet.routingtable.default_table | Body | Boolean | 라우팅 테이블의 Default Table 여부 |
| vpcsubnet.routingtable.explicit      | Body | Boolean | 라우팅 테이블과의 명시적 연결 여부       |
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
| vpcsubnet.routes.tenant_id           | Body | UUID    | 정적 경로의 테넌트 ID             |
| vpcsubnet.routes.mask                | Body | Integer | 정적 경로의 마스크                |
| vpcsubnet.routes.gateway             | Body | String  | 정적 경로의 게이트웨이 IP           |
| vpcsubnet.routes.cidr                | Body | String  | 정적 경로의 대상 IP 대역           |
| vpcsubnet.routes.id                  | Body | UUID    | 정적 경로의 ID                 |
| vpcsubnet.shared                     | Body | Boolean | 서브넷의 공유 여부                |
| vpcsubnet.cidr                       | Body | String  | 서브넷의 IP 대역                |
| vpcsubnet.gateway                    | Body | String  | 서브넷의 게이트웨이 IP             |


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
| vpcsubnet.tenant_id | Body   | UUID   |     | 서브넷이 할당될 테넌트 ID |

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
| routingtable_id | Body | UUID | O   | 연결할 라우팅 테이블 ID |

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
| vpcsubnet.routingtable               | Body | Object  | 서브넷이 연결된 라우팅 테이블          |
| vpcsubnet.routingtable.gateway_id    | Body | UUID    | 라우팅 테이블의 게이트웨이 ID         |
| vpcsubnet.routingtable.default_table | Body | Boolean | 라우팅 테이블의 Default Table 여부 |
| vpcsubnet.routingtable.explicit      | Body | Boolean | 라우팅 테이블과의 명시적 연결 여부       |
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
| vpcsubnet.routes.tenant_id           | Body | UUID    | 정적 경로의 테넌트 ID             |
| vpcsubnet.routes.mask                | Body | Integer | 정적 경로의 마스크                |
| vpcsubnet.routes.gateway             | Body | String  | 정적 경로의 게이트웨이 IP           |
| vpcsubnet.routes.cidr                | Body | String  | 정적 경로의 대상 IP 대역           |
| vpcsubnet.routes.id                  | Body | UUID    | 정적 경로의 ID                 |
| vpcsubnet.shared                     | Body | Boolean | 서브넷의 공유 여부                |
| vpcsubnet.cidr                       | Body | String  | 서브넷의 IP 대역                |
| vpcsubnet.gateway                    | Body | String  | 서브넷의 게이트웨이 IP             |



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
서브넷과 라우팅 테이블의 명시적 연결을 해제합니다.

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

| 이름                                   | 종류   | 형식      | 설명                       |
|--------------------------------------|------|---------|--------------------------|
| vpcsubnet                            | Body | Array   | 서브넷 정보 객체                |
| vpcsubnet.router:external            | Body | Boolean | 서브넷의 외부 연결 여부            |
| vpcsubnet.name                       | Body | String  | 서브넷의 이름                  |
| vpcsubnet.enable_dhcp                | Body | Boolean | 서브넷의 DHCP 활성화 여부         |
| vpcsubnet.tenant_id                  | Body | UUID    | 서브넷이 속한 테넌트 ID           |
| vpcsubnet.routingtable               | Body | Object  | 서브넷의 라우팅 테이블             |
| vpcsubnet.routingtable.gateway_id    | Body | UUID    | 라우팅 테이블의 게이트웨이 ID        |
| vpcsubnet.routingtable.default_table | Body | Boolean | 라우팅 테이블의 Default Table 여부 |
| vpcsubnet.routingtable.explicit      | Body | Boolean | 라우팅 테이블의 명시적 연결 여부       |
| vpcsubnet.routingtable.id            | Body | UUID    | 라우팅 테이블의 ID              |
| vpcsubnet.routingtable.name          | Body | String  | 라우팅 테이블의 이름              |
| vpcsubnet.gateway                    | Body | String  | 서브넷에 연결된 게이트웨이 IP        |
| vpcsubnet.routes                     | Body | Array   | 서브넷의 정적 경로               |
| vpcsubnet.routes.subnet_id           | Body | UUID    | 정적 경로의 서브넷 ID            |
| vpcsubnet.routes.tenant_id           | Body | UUID    | 정적 경로의 테넌트 ID            |
| vpcsubnet.routes.mask                | Body | Integer | 정적 경로의 마스크               |
| vpcsubnet.routes.gateway             | Body | String  | 정적 경로의 게이트웨이 IP          |
| vpcsubnet.routes.cidr                | Body | String  | 정적 경로의 IP 대역             |
| vpcsubnet.routes.id                  | Body | UUID    | 정적 경로의 ID                |
| vpcsubnet.state                      | Body | String  | 서브넷의 상태                  |
| vpcsubnet.create_time                | Body | Date    | 서브넷의 생성 시간               |
| vpcsubnet.available_ip_count         | Body | Integer | 서브넷이 사용 가능한 IP 개수        |
| vpcsubnet.vpc                        | Body | Object  | 서브넷이 속한 VPC              |
| vpcsubnet.vpc.shared                 | Body | Boolean | VPC의 공유 여부               |
| vpcsubnet.vpc.state                  | Body | String  | VPC의 상태                  |
| vpcsubnet.vpc.id                     | Body | UUID    | VPC의 ID                  |
| vpcsubnet.vpc.cidrv4                 | Body | String  | VPC의 IP 대역               |
| vpcsubnet.vpc.name                   | Body | String  | VPC 이름                   |
| vpcsubnet.shared                     | Body | Boolean | 서브넷의 공유 여부               |
| vpcsubnet.id                         | Body | UUID    | 서브넷의 ID                  |
| vpcsubnet.vpc_id                     | Body | UUID    | 서브넷이 속한 VPC의 ID          |
| vpcsubnet.cidr                       | Body | String  | 서브넷의 IP 대역               |


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


## 라우팅 테이블

### 라우팅 테이블 목록 보기

사용 가능한 라우팅 테이블의 목록을 반환합니다.

```
GET /v2.0/routingtables
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름       | 종류     | 형식     | 필수  | 설명     |
|----------|--------|--------|-----|--------|
| tokenId | Header | String | O | 토큰 ID |
| tenant_id | Query | String | - | 조회할 라우팅 테이블이 속한 테넌트 ID |
| id | Query | UUID | - | 조회할 라우팅 테이블 ID |
| name | Query | String | - | 조회할 라우팅 테이블 이름 |
| default_table | Query | Boolean | - | 조회할 라우팅 테이블의 기본 라우팅 테이블 여부 |
| gateway_id | Query | UUID | - | 조회할 라우팅 테이블에 연결된 인터넷 게이트웨이 ID |
| distributed | Query | Boolean | - | 조회할 라우팅 테이블의 라우팅 방식<br>`true`: 분산형, `false`: 중앙 집중형 | 
| detail | Query | Boolean | - | 조회할 라우팅 테이블의 상세 정보 포함 여부 |
| sort_dir | Query  | Enum    | -   | 조회할 네트워크의 정렬 방향<br>`sort_key`에서 지정한 필드를 기준으로 정렬<br>**asc**, **desc** 중 하나 |
| sort_key | Query  | String  | -   | 조회할 네트워크의 정렬 키<br>`sort_dir`에서 지정한 방향대로 정렬 |

#### 응답

##### **detail** 쿼리 파라미터가 없거나 값이 `false`일 때

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| routingtables | Body | Array | 라우팅 테이블 정보 객체 목록 |
| routingtables.id | Body | UUID | 라우팅 테이블 ID |
| routingtables.name | Body | String | 라우팅 테이블 이름 |
| routingtables.default_table | Body | Boolean | 기본 라우팅 테이블 여부 |
| routingtables.distributed | Body | Boolean | 조회할 라우팅 테이블의 라우팅 방식<br>`true`: 분산형, `false`: 중앙 집중형 |
| routingtables.gateway_id | Body | UUID | 라우팅 테이블에 인터넷 게이트웨분산형이가 연결된 경우 해당 인터넷 게이트웨이의 ID |
| routingtables.tenant_id | Body | String | 라우팅 테이블이 속한 테넌트 ID |
| routingtables.state | Body | String | 라우팅 테이블의 상태. 현재는 `available` 상태만 존재 |
| routingtable.create_time | Date | 라우팅 테이블의 생성 시간 |

<details><summary>예시</summary>
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

##### **detail** 쿼리 파라미터 값이 `true`일 때

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| routingtables | Body | Array | 라우팅 테이블 정보 객체 목록 |
| routingtables.id | Body | UUID | 라우팅 테이블 ID |
| routingtables.name | Body | String | 라우팅 테이블 이름 |
| routingtables.default_table | Body | Boolean | 기본 라우팅 테이블 여부 |
| routingtables.distributed | Body | Boolean | 조회할 라우팅 테이블의 라우팅 방식<br>`true`: 분산형, `false`: 중앙 집중형 |
| routingtables.gateway_id | Body | UUID | 라우팅 테이블에 인터넷 게이트웨이가 연결된 경우 해당 인터넷 게이트웨이의 ID |
| routingtables.tenant_id | Body | String | 라우팅 테이블이 속한 테넌트 ID |
| routingtables.state | Body | String | 라우팅 테이블의 상태. 현재는 `available` 상태만 존재 |
| routingtables.vpcs | Body | Array | 라우팅 테이블이 속한 VPC 정보 객체 목록 |
| routingtables.vpcs.id | Body | UUID | 라우팅 테이블이 속한 VPC ID |
| routingtables.vpcs.name | Body | String | 라우팅 테이블이 속한 VPC 이름 |
| routingtables.subnets | Body | Array | 라우팅 테이블에 연결된 서브넷 정보 객체 목록 | 
| routingtables.subnets.id | Body | UUID | 라우팅 테이블에 연결된 서브넷 ID | 
| routingtables.subnet.name | Body | UUID | 라우팅 테이블에 연결된 서브넷 이름 |
| routingtables.create_time | Date | 라우팅 테이블의 생성 시간 |

<details><summary>예시</summary>
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

### 라우팅 테이블 보기

지정한 라우팅 테이블을 조회합니다.

```
GET /v2.0/routingtables/{routingtableId}
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| routingtableId | URL | UUID | O | 조회할 라우팅 테이블 ID |
| tokenId | Header | String | O | 토큰 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| routingtable | Body | Object | 포트 정보 객체 |
| routingtable.id | Body | UUID | 라우팅 테이블 ID |
| routingtable.name | Body | String | 라우팅 테이블 이름 |
| routingtable.default_table | Body | Boolean | 기본 라우팅 테이블 여부 |
| routingtable.distributed | Body | Boolean | 조회할 라우팅 테이블의 라우팅 방식<br>`true`: 분산형, `false`: 중앙 집중형 |
| routingtable.gateway_id | Body | UUID | 라우팅 테이블에 인터넷 게이트웨이가 연결된 경우 해당 인터넷 게이트웨이의 ID |
| routingtable.tenant_id | Body | String | 라우팅 테이블이 속한 테넌트 ID |
| routingtable.state | Body | String | 라우팅 테이블의 상태. 현재는 `available` 상태만 존재 |
| routingtable.vpcs | Body | Array | 라우팅 테이블이 속한 VPC 정보 객체 목록 |
| routingtable.vpcs.id | Body | UUID | 라우팅 테이블이 속한 VPC ID |
| routingtable.subnets | Body | Array | 라우팅 테이블에 연결된 서브넷 정보 객체 목록 | 
| routingtable.subnets.id | Body | UUID | 라우팅 테이블에 연결된 서브넷 ID | 
| routingtable.routes | Body | Array | 라우팅 테이블에 설정된 라우트 정보 객체 목록 |
| routingtable.routes.id | Body | UUID | 라우트 ID |
| routingtable.routes.cidr | Body | String | 라우트 목적지 CIDR |
| routingtable.routes.mask | Body | String | 라우트 목적지 CIDR의 넷마스크 |
| routingtable.routes.gateway | Body | String | 라우트 게이트웨이 IP |
| routingtable.routes.gateway_id | Body | String | 인터넷 게이트웨이로 향하는 라우트의 경우 인터넷 게이트웨이 ID |
| routingtable.routes.routingtable_id | Body | String | 라우트가 속한 라우팅 테이블 ID |
| routingtable.routes.tenant_id | Body | String | 라우트가 속한 테넌트 ID |
| routingtable.create_time | Date | 라우팅 테이블의 생성 시간 |

<details><summary>예시</summary>
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

### 라우팅 테이블 생성하기

새로운 라우팅 테이블을 생성합니다.

```
POST /v2.0/routingtables
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | 토큰 ID |
| routingtable | Body | Object | O | 라우팅 테이블 생성 요청 객체 |
| routingtable.name | Body | String | O | 라우팅 테이블 이름 |
| routingtable.vpc_id | Body | UUID | O | 라우팅 테이블이 속할 VPC ID |
| routingtable.distributed | Body | Boolean | - | 라우팅 테이블의 라우팅 방식<br>`true`: 분산형, `false`: 중앙 집중형 (기본값: `true`) |

<details><summary>예시</summary>
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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| routingtable | Body | Object | 생성된 라우팅 테이블 정보 객체 |
| id | Body | UUID | 라우팅 테이블 ID |
| name | Body | String | 라우팅 테이블 이름 |
| default_table | Body | Boolean | 기본 라우팅 테이블 여부 |
| distributed | Body | Boolean | 라우팅 테이블의 라우팅 방식<br>`true`: 분산형, `false`: 중앙 집중형 |
| state | Body | String | 라우팅 테이블의 상태. 현재는 `"available"` 값만 사용 |
| tenant_id | Body | String | 라우팅 테이블이 속한 테넌트 ID |
| create_time | Body | Date | 라우팅 테이블 생성 시간 |

<details><summary>예시</summary>
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

### 라우팅 테이블 수정하기

라우팅 테이블의 정보를 수정합니다. 라우팅 테이블의 이름과 라우팅 방식(분산형/중앙 집중형)을 변경할 수 있습니다.

```
PUT /v2.0/routingtables/{routingtableId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| routingtableId | URL | UUID | O | 수정할 라우팅 테이블 ID |
| tokenId | Header | String | O | 토큰 ID |
| routingtable | Body | Object | O | 라우팅 테이블 정보 객체 |
| routingtable.name | Body | String | - | 변경할 라우팅 테이블 이름 |
| routingtable.distributed | Body | Boolean | - | 변경할 라우팅 테이블의 라우팅 방식<br>`true`: 분산형, `false`: 중앙 집중형 | 


<details><summary>예시</summary>
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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| routingtable | Body | Object | 생성된 라우팅 테이블 정보 객체 |
| id | Body | UUID | 라우팅 테이블 ID |
| name | Body | String | 라우팅 테이블 이름 |
| default_table | Body | Boolean | 기본 라우팅 테이블 여부 |
| distributed | Body | Boolean | 라우팅 테이블의 라우팅 방식<br>`true`: 분산형, `false`: 중앙 집중형 |
| state | Body | String | 라우팅 테이블의 상태. 현재는 `"available"` 값만 사용 |
| tenant_id | Body | String | 라우팅 테이블이 속한 테넌트 ID |
| create_time | Body | Date | 라우팅 테이블 생성 시간 |

<details><summary>예시</summary>
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


### 라우팅 테이블에 인터넷 게이트웨이 연결하기

라우팅 테이블에 인터넷 게이트웨이를 연결합니다.

```
PUT /v2.0/routingtables/{routingtableId}/attach_gateway
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| routingtableId | URL | UUID | O | 수정할 라우팅 테이블 ID |
| tokenId | Header | String | O | 토큰 ID |
| gateway_id | Body | UUID | O | 라우팅 테이블에 연결할 인터넷 게이트웨이의 ID |

<details><summary>예시</summary>
<p>

```json
{
  "gateway_id": "615d1cb1-fe54-4505-8a39-35faa6c868cd"
}
```

</p>
</details>

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| routingtable | Body | Object | 포트 정보 객체 |
| routingtable.id | Body | UUID | 라우팅 테이블 ID |
| routingtable.name | Body | String | 라우팅 테이블 이름 |
| routingtable.default_table | Body | Boolean | 기본 라우팅 테이블 여부 |
| routingtable.distributed | Body | Boolean | 조회할 라우팅 테이블의 라우팅 방식<br>`true`: 분산형, `false`: 중앙 집중형 |
| routingtable.gateway_id | Body | UUID | 라우팅 테이블에 인터넷 게이트웨이가 연결된 경우 해당 인터넷 게이트웨이의 ID |
| routingtable.tenant_id | Body | String | 라우팅 테이블이 속한 테넌트 ID |
| routingtable.state | Body | String | 라우팅 테이블의 상태. 현재는 `available` 상태만 존재 |
| routingtable.vpcs | Body | Array | 라우팅 테이블이 속한 VPC 정보 객체 목록 |
| routingtable.vpcs.id | Body | UUID | 라우팅 테이블이 속한 VPC ID |
| routingtable.subnets | Body | Array | 라우팅 테이블에 연결된 서브넷 정보 객체 목록 | 
| routingtable.subnets.id | Body | UUID | 라우팅 테이블에 연결된 서브넷 ID | 
| routingtable.routes | Body | Array | 라우팅 테이블에 설정된 라우트 정보 객체 목록 |
| routingtable.routes.id | Body | UUID | 라우트 ID |
| routingtable.routes.cidr | Body | String | 라우트 목적지 CIDR |
| routingtable.routes.mask | Body | String | 라우트 목적지 CIDR의 넷마스크 |
| routingtable.routes.gateway | Body | String | 라우트 게이트웨이 IP |
| routingtable.routes.gateway_id | Body | String | 인터넷 게이트웨이로 향하는 라우트의 경우 인터넷 게이트웨이 ID |
| routingtable.routes.routingtable_id | Body | String | 라우트가 속한 라우팅 테이블 ID |
| routingtable.routes.tenant_id | Body | String | 라우트가 속한 테넌트 ID |
| routingtable.create_time | Date | 라우팅 테이블의 생성 시간 |

<details><summary>예시</summary>
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

### 라우팅 테이블의 인터넷 게이트웨이 연결 해제하기

라우팅 테이블과 연결된 인터넷 게이트웨이와의 연결을 해제합니다.

```
PUT /v2.0/routingtables/{routingtableId}/detach_gateway
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| routingtableId | URL | UUID | O | 수정할 라우팅 테이블 ID |
| tokenId | Header | String | O | 토큰 ID |


#### 응답
| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| routingtable | Body | Object | 포트 정보 객체 |
| routingtable.id | Body | UUID | 라우팅 테이블 ID |
| routingtable.name | Body | String | 라우팅 테이블 이름 |
| routingtable.default_table | Body | Boolean | 기본 라우팅 테이블 여부 |
| routingtable.distributed | Body | Boolean | 조회할 라우팅 테이블의 라우팅 방식<br>`true`: 분산형, `false`: 중앙 집중형 |
| routingtable.tenant_id | Body | String | 라우팅 테이블이 속한 테넌트 ID |
| routingtable.state | Body | String | 라우팅 테이블의 상태. 현재는 `available` 상태만 존재 |
| routingtable.vpcs | Body | Array | 라우팅 테이블이 속한 VPC 정보 객체 목록 |
| routingtable.vpcs.id | Body | UUID | 라우팅 테이블이 속한 VPC ID |
| routingtable.subnets | Body | Array | 라우팅 테이블에 연결된 서브넷 정보 객체 목록 | 
| routingtable.subnets.id | Body | UUID | 라우팅 테이블에 연결된 서브넷 ID | 
| routingtable.routes | Body | Array | 라우팅 테이블에 설정된 라우트 정보 객체 목록 |
| routingtable.routes.id | Body | UUID | 라우트 ID |
| routingtable.routes.cidr | Body | String | 라우트 목적지 CIDR |
| routingtable.routes.mask | Body | String | 라우트 목적지 CIDR의 넷마스크 |
| routingtable.routes.gateway | Body | String | 라우트 게이트웨이 IP |
| routingtable.routes.gateway_id | Body | String | 인터넷 게이트웨이로 향하는 라우트의 경우 인터넷 게이트웨이 ID |
| routingtable.routes.routingtable_id | Body | String | 라우트가 속한 라우팅 테이블 ID |
| routingtable.routes.tenant_id | Body | String | 라우트가 속한 테넌트 ID |
| routingtable.create_time | Date | 라우팅 테이블의 생성 시간 |

<details><summary>예시</summary>
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

### 라우팅 테이블을 기본 라우팅 테이블로 지정하기

라우팅 테이블을 기본 라우팅 테이블로 지정합니다.

```
PUT /v2.0/routingtables/{routingtableId}/set_as_default
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| routingtableId | URL | UUID | O | 수정할 라우팅 테이블 ID |
| tokenId | Header | String | O | 토큰 ID |


#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| routingtable | Body | Object | 포트 정보 객체 |
| routingtable.id | Body | UUID | 라우팅 테이블 ID |
| routingtable.name | Body | String | 라우팅 테이블 이름 |
| routingtable.default_table | Body | Boolean | 기본 라우팅 테이블 여부 |
| routingtable.distributed | Body | Boolean | 조회할 라우팅 테이블의 라우팅 방식<br>`true`: 분산형, `false`: 중앙 집중형 |
| routingtable.gateway_id | Body | UUID | 라우팅 테이블에 인터넷 게이트웨이가 연결된 경우 해당 인터넷 게이트웨이의 ID |
| routingtable.tenant_id | Body | String | 라우팅 테이블이 속한 테넌트 ID |
| routingtable.state | Body | String | 라우팅 테이블의 상태. 현재는 `available` 상태만 존재 |
| routingtable.vpcs | Body | Array | 라우팅 테이블이 속한 VPC 정보 객체 목록 |
| routingtable.vpcs.id | Body | UUID | 라우팅 테이블이 속한 VPC ID |
| routingtable.subnets | Body | Array | 라우팅 테이블에 연결된 서브넷 정보 객체 목록 | 
| routingtable.subnets.id | Body | UUID | 라우팅 테이블에 연결된 서브넷 ID | 
| routingtable.routes | Body | Array | 라우팅 테이블에 설정된 라우트 정보 객체 목록 |
| routingtable.routes.id | Body | UUID | 라우트 ID |
| routingtable.routes.cidr | Body | String | 라우트 목적지 CIDR |
| routingtable.routes.mask | Body | String | 라우트 목적지 CIDR의 넷마스크 |
| routingtable.routes.gateway | Body | String | 라우트 게이트웨이 IP |
| routingtable.routes.gateway_id | Body | String | 인터넷 게이트웨이로 향하는 라우트의 경우 인터넷 게이트웨이 ID |
| routingtable.routes.routingtable_id | Body | String | 라우트가 속한 라우팅 테이블 ID |
| routingtable.routes.tenant_id | Body | String | 라우트가 속한 테넌트 ID |
| routingtable.create_time | Date | 라우팅 테이블의 생성 시간 |

<details><summary>예시</summary>
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


### 라우팅 테이블 삭제하기

라우팅 테이블을 삭제합니다. 기본 라우팅 테이블로 지정된 라우팅 테이블은 삭제할 수 없으며, 이는 VPC 삭제 시 함께 삭제됩니다.

```
DELETE /v2.0/routingtables/{routingtableId}
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| routingtableId | URL | UUID | O | 삭제할 라우팅 테이블 ID |
| tokenId | Header | String | O | 토큰 ID |


#### 응답

이 API는 응답 본문을 반환하지 않습니다.

## 라우트
### 라우트 목록 보기

라우팅 테이블에 설정된 라우트 목록을 반환합니다.

```
GET /v2.0/routes
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | 토큰 ID |
| id | Query | UUID | - | 라우트 ID |
| cidr | Query | String | - | 라우트 목적지 CIDR |
| mask | Query | Integer | -  | 라우트 목적지 CIDR의 넷마스크(0~32) |
| gateway | Query | String | - | 라우트 게이트웨이 IP |
| routingtable_id |  Query | String | - | 라우트가 설정된 라우팅 테이블의 ID|
| gateway_id |  Query | String | - | 인터넷 게이트웨이 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| routes | Body | Array | 라우트 정보 객체 목록 |
| routes.id | Body | UUID | 라우트 ID |
| routes.cidr | Body | String | 라우트 목적지 CIDR | 
| routes.mask | Body | Integer | 라우트 목적지 CIDR의 넷마스크 |
| routes.gateway | Body | String | 라우트 게이트웨이 IP. 로컬 라우트인 경우 "local" |
| routes.gateway_id | Body | UUID | 라우팅 테이블에 인터넷 게이트웨이를 연결하여 자동으로 생성된 라우트인 경우, 인터넷 게이트웨이의 ID |
| routes.tenant_id | Body | String | 라우트가 속한 테넌트 ID |

<details><summary>예시</summary>
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

### 라우트 보기

지정한 라우트를 조회합니다.

```
GET /v2.0/routes/{routeId}
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| routeId | URL | UUID | O | 조회할 라우트 ID |
| tokenId | Header | String | O | 토큰 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| route | Body | Array | 라우트 정보 객체 |
| route.id | Body | UUID | 라우트 ID |
| route.cidr | Body | String | 라우트 목적지 CIDR | 
| route.mask | Body | Integer | 라우트 목적지 CIDR의 넷마스크 |
| route.gateway | Body | String | 라우트 게이트웨이 IP. 로컬 라우트인 경우 "local" |
| route.gateway_id | Body | UUID | 라우팅 테이블에 인터넷 게이트웨이를 연결하여 자동으로 생성된 라우트인 경우, 인터넷 게이트웨이의 ID |
| route.tenant_id | Body | String | 라우트가 속한 테넌트 ID |

<details><summary>예시</summary>
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


### 라우트 생성하기

라우팅 테이블에 신규 라우트를 추가합니다.

```
POST /v2.0/routes
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | 토큰 ID |
| routingtable_id | Body | UUID | O | 라우트를 추가할 라우팅 테이블 ID |
| cidr | Body | String | O | 라우트 목적지 CIDR |
| gateway| Body | String | O | 라우트 게이트웨이 IP |

<details><summary>예시</summary>
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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
| --- | --- | --- | --- |
| route | Body | Array | 라우트 정보 객체 |
| route.id | Body | UUID | 라우트 ID |
| route.cidr | Body | String | 라우트 목적지 CIDR | 
| route.mask | Body | Integer | 라우트 목적지 CIDR의 넷마스크 |
| route.gateway | Body | String | 라우트 게이트웨이 IP |
| route.tenant_id | Body | String | 라우트가 속한 테넌트 ID |

<details><summary>예시</summary>
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

### 라우트 삭제하기

지정한 라우트를 삭제합니다. `gateway` 항목이 "local" 이거나, 인터넷 게이트웨이 연결로 인해 자동으로 추가된 라우트(`gateway_id` 값 존재)는 삭제할 수 없습니다.

```
DELETE /v2.0/routes/{routeId}
X-Auth-Token: {tokenId}
```

#### 요청

이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
| --- | --- | --- | --- | --- |
| routeId | URL | UUID | O | 삭제할 라우트 ID |
| tokenId | Header | String | O | 토큰 ID |


#### 응답

이 API는 응답 본문을 반환하지 않습니다.


---