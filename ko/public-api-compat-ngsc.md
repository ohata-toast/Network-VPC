## Network > VPC > API v2 가이드

API를 사용하려면 API 엔드포인트와 토큰 등이 필요합니다. [API 사용 준비](/Compute/Compute/ko/identity-api-gov/)를 참고하여 API 사용에 필요한 정보를 준비합니다.

VPC API는 `network` 타입 엔드포인트를 이용합니다. 정확한 엔드포인트는 토큰 발급 응답의 `serviceCatalog`를 참조합니다.

| 타입 | 리전 | 엔드포인트 |
|---|---|---|
| network | 한국(판교) 리전 | https://kr1-api-network-infrastructure.gov-nhncloudservice.com |

API 응답에 가이드에 명시되지 않은 필드가 나타날 수 있습니다. 이런 필드는 NHN Cloud 내부 용도로 사용되며 사전 공지 없이 변경될 수 있으므로 사용하지 않습니다.

## 네트워크
### 네트워크 목록 보기
사용 가능한 네트워크 목록을 반환합니다.
```
GET /v2.0/networks
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| id | Query | UUID | - | 조회할 네트워크 ID |
| name | Query | String | - | 조회할 네트워크 이름 |
| provider:network_type | Query | Enum | - | 조회할 네트워크 타입<br>`flat`, `vlan` 중 하나 |
| router:external | Query | Boolean | - | 조회할 네트워크의 외부 연결 여부 |
| shared | Query | Boolean | - | 조회할 네트워크의 공유 여부 |
| tenant_id | Query | String | - | 조회할 네트워크가 속한 테넌트 ID |
| sort_dir | Query | Enum | - | 조회할 네트워크의 정렬 방향<br>`sort_key`에서 지정한 필드를 기준으로 정렬<br>**asc**, **desc** 중 하나 |
| sort_key | Query | String | - | 조회할 네트워크의 정렬 키<br>`sort_dir`에서 지정한 방향대로 정렬 |
| fields | Query | String | - | 조회할 네트워크의 필드 이름<br>예) `fields=id&fields=name` |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| networks | Body | Array | 네트워크 정보 객체 목록 |
| networks.status | Body | Enum | 네트워크 상태<br>**ACTIVE**, **DOWN**, **BUILD**, **ERROR** 중 하나. |
| networks.subnets | Body | Array | 네트워크에 속한 서브넷들의 ID 목록 |
| networks.name | Body | String | 네트워크 이름 |
| networks.router:external | Body | Boolean | 네트워크 외부 연결 여부 |
| networks.tenant_id | Body | String | 테넌트 ID |
| networks.admin_state_up | Body | Boolean | 관리자 제어 상태<br>`true`: 사용 가능<br>`false`: 사용 불가 |
| networks.mtu | Body | Integer | 최대 전송 단위(Maximum Transmission Unit) |
| networks.shared | Body | Boolean | 네트워크 공유 여부 |
| networks.port_security_enabled | Body | Boolean | 네트워크 포트 보안 여부<br>이 네트워크에서 생성되는 포트의 보안 활성화 여부를 결정 |
| networks.id | Body | String | 네트워크 ID |
| networks.name | Body | String | 네트워크 이름 |
| networks_links | Body | Array | 페이지 매김(페이지네이션)을 위한 정보 객체<br>`limit`, `offset`를 추가한 경우 반환<br>다음 목록을 가리키는 경로를 포함 |

<details><summary>예시</summary>
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

## 서브넷
### 서브넷 목록 보기
사용 가능한 서브넷 목록을 반환합니다.
```
GET /v2.0/subnets
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| id | Query | UUID | - | 조회할 서브넷 ID |
| name | Query | String | - | 조회할 서브넷 이름 |
| enable_dhcp | Query | Boolean | - | 조회할 서브넷의 DHCP 활성화 여부 |
| network_id | Query | UUID | - | 조회할 서브넷의 네트워크 ID |
| cidr | Query | String | - | 조회할 서브넷 CIDR |
| shared | Query | Boolean | - | 조회할 서브넷의 공유 여부 |
| sort_dir | Query | Enum | - | 조회할 서브넷의 정렬 방향<br>`sort_key`에서 지정한 필드를 기준으로 정렬<br>**asc**, **desc** 중 하나 |
| sort_key | Query | String | - | 조회할 서브넷의 정렬 키<br>`sort_dir`에서 지정한 방향대로 정렬 |
| fields | Query | String | - | 조회할 서브넷의 필드 이름<br>예) `fields=id&fields=name` |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| subnets | Body | Array | 서브넷 정보 객체 목록 |
| subnets.name | Body | String | 서브넷 이름 |
| subnets.enable_dhcp | Body | Boolean | 서브넷의 DHCP 활성화 여부 |
| subnets.network_id | Body | UUID | 서브넷의 네트워크 ID |
| subnets.tenant_id | Body | String | 테넌트 ID |
| subnets.dns_nameservers | Body | Array | 서브넷에 연결괸 DNS 네임서버 목록 |
| subnets.gateway_ip | Body | String | 서브넷의 게이트웨이 IP |
| subnets.ipv6_ra_mode | Body | Boolean | IPv6의 Router Advertisement 모드 |
| subnets.allocation_pools | Body | Array | 서브넷 IP 범위 객체 목록 |
| subnets.allocation_pools.start | Body | String | 서브넷 IP 범위의 시작 IP 주소 |
| subnets.allocation_pools.end | Body | String | 서브넷 IP 범위의 마지막 IP 주소 |
| subnets.host_routes | Body | Array | 서브넷의 추가 경로 정보 목록 |
| subnets.host_routes.destination | Body | String | 목적지<br>목적지 주소가 `destination`이면<br>`nexthop`으로 지정된 주소로 전달 |
| subnets.host_routes.nexthop | Body | String | 다음 hop 주소 |
| subnets.ip_version | Body | Integer | IP 프로토콜 버전<br>4 또는 6 |
| subnets.ipv6_address_mode | Body | String | IPv6의 주소 할당 모드 |
| subnets.cidr | Body | String | 서브넷의 CIDR |
| subnets.id | Body | UUID | 서브넷의 ID |
| subnets.subnetpool_id | Body | UUID | 서브넷 Pool ID |
| subnets_links | Body | Array | 페이지 매김(페이지네이션)을 위한 정보 객체<br>`limit`, `offset`를 추가한 경우 반환<br>다음 목록을 가리키는 경로를 포함 |

<details><summary>예시</summary>
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

## 포트
### 포트 목록 보기
포트 목록을 반환합니다.
```
GET /v2.0/ports
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| id | Query | UUID | - | 조회할 포트 IP ID |
| status | Query | Enum | - | 조회할 포트 상태<br>**ACTIVE**, **DOWN** 중 하나. |
| display_name | Query | UUID | - | 조회할 포트 이름 |
| admin_state | Query | Boolean | - | 조회할 포트의 관리자 제어 상태 |
| network_id | Query | UUID | - | 조회할 포트의 네트워크 ID |
| tenant_id | Query | String | - | 조회할 포트의 테넌트 ID |
| device_owner | Query | String | - | 조회할 포트를 사용하는 리소스 종류 |
| mac_address | Query | String | - | 조회할 포트의 MAC 주소 |
| port_id | Query | UUID | - | 조회할 포트 ID |
| security_groups | Query | UUID | - | 조회할 포트의 보안 그룹 ID |
| device_id | Query | UUID | - | 조회할 포트를 사용하는 리소스 ID |
| fields | Query | String | - | 조회할 포트의 필드 이름<br>예) `fields=id&fields=name` |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| ports | Body | Array | 포트 정보 객체 목록 |
| ports.status | Body | Enum | 포트 상태<br>**ACTIVE**, **DOWN** 중 하나. |
| ports.name | Body | String | 포트 이름 |
| ports.allowed_address_pairs | Body | Array | 포트의 주소 쌍 목록 |
| ports.admin_state_up | Body | Boolean | 포트의 관리자 제어 상태 |
| ports.network_id | Body | UUID | 포트의 네트워크 ID |
| ports.tenant_id | Body | String | 테넌트 ID |
| ports.extra_dhcp_opts | Body | Array | 추가 DHCP 설정 |
| ports.binding:vnic_type | Body | String | 포트 타입 |
| ports.device_owner | Body | String | 포트를 사용하는 리소스 종류 |
| ports.mac_address | Body | String | 포트의 MAC 주소 |
| ports.port_security_enabled | Body | Boolean | 포트의 보안 상태<br>활성화된 경우 보안 그룹 적용 가능 |
| ports.fixed_ips | Body | Array | 포트의 고정 IP 목록 |
| ports.fixed_ips.subnet_id | Body | UUID | 포트의 고정 IP의 서브넷 ID |
| ports.fixed_ips.ip_address | Body | String | 포트의 고정 IP 주소 |
| ports.id | Body | UUID | 포트의 ID |
| ports.security_groups | Body | Array | 포트의 보안 그룹 ID 목록 |
| ports.device_id | Body | UUID | 포트를 사용하는 리소스 ID |

<details><summary>예시</summary>
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

### 포트 보기

```
GET /v2.0/ports/{portId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| portId | URL | UUID | O | 포트 ID |
| tokenId | Header | String | O | 토큰 ID |
| fields | Query | String | - | 조회할 포트의 필드 이름<br>예) `fields=id&fields=name` |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| port | Body | Object | 포트 정보 객체 |
| port.status | Body | Enum | 포트 상태<br>**ACTIVE**, **DOWN** 중 하나 |
| port.name | Body | String | 포트 이름 |
| port.allowed_address_pairs | Body | Array | 포트의 주소 쌍 목록 |
| port.admin_state_up | Body | Boolean | 포트의 관리자 제어 상태 |
| port.network_id | Body | UUID | 포트의 네트워크 ID |
| port.tenant_id | Body | String | 테넌트 ID |
| port.extra_dhcp_opts | Body | Array | 추가 DHCP 설정 |
| port.binding:vnic_type | Body | String | 포트 타입 |
| port.device_owner | Body | String | 포트를 사용하는 리소스 종류 |
| port.mac_address | Body | String | 포트의 MAC 주소 |
| port.port_security_enabled | Body | Boolean | 포트의 보안 상태<br>활성화된 경우 보안 그룹 적용 가능 |
| port.fixed_ips | Body | Array | 포트의 고정 IP 목록 |
| port.fixed_ips.subnet_id | Body | UUID | 포트의 고정 IP의 서브넷 ID |
| port.fixed_ips.ip_address | Body | String | 포트의 고정 IP 주소 |
| port.id | Body | UUID | 포트의 ID |
| port.security_groups | Body | Array | 포트의 보안 그룹 ID 목록 |
| port.device_id | Body | UUID | 포트를 사용하는 리소스 ID |

<details><summary>예시</summary>
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

### 포트 생성 하기
새로운 포트를 생성합니다. 생성한 포트는 인스턴스 생성 시 활용할 수 있습니다.
```
POST /v2.0/ports
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| port | Body | Object | O | 포트 생성 요청 객체 |
| port.name | Body | String | - | 포트 이름 |
| port.network_id | Body | UUID | O | 포트의 네트워크 ID |
| port.admin_state_up | Body | Boolean | - | 포트의 관리자 제어 상태 |
| port.mac_address | Body | String | - | 포트의 MAC 주소 |
| port.port_id | Body | UUID | - | 플로팅 IP가 연결된 포트 ID |
| port.fixed_ips | Body | Array | - | 포트의 고정 IP 목록 |
| port.fixed_ips.subnet_id | Body | UUID | - | 포트의 고정 IP의 서브넷 ID |
| port.fixed_ips.ip_address | Body | String | - | 포트의 고정 IP 주소 |
| port.security_groups | Body | Array | - | 포트의 보안 그룹 ID 목록 |
| port.allowed_address_pairs | Body | Array | - | 포트의 주소 쌍 목록 |
| port.allowed_address_pairs.ip_address | Body | String | - | 포트의 IP 주소 |
| port.allowed_address_pairs.mac_address | Body | String | - | 포트의 MAC 주소 |
| port.extra_dhcp_opts | Body | Array | - | 추가 DHCP 설정 |
| port.device_owner | Body | String | - | 포트를 사용하는 리소스 종류 |
| port.device_id | Body | UUID | - | 포트를 사용하는 리소스 ID |

<details><summary>예시</summary>
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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| port | Body | Array | 포트 정보 객체 |
| port.status | Body | Enum | 포트 상태<br>**ACTIVE**, **DOWN** 중 하나 |
| port.name | Body | String | 포트 이름 |
| port.allowed_address_pairs | Body | Array | 포트의 주소 쌍 목록 |
| port.admin_state_up | Body | Boolean | 포트의 관리자 제어 상태 |
| port.network_id | Body | UUID | 포트의 네트워크 ID |
| port.tenant_id | Body | String | 테넌트 ID |
| port.extra_dhcp_opts | Body | Array | 추가 DHCP 설정 |
| port.binding:vnic_type | Body | String | 포트 타입 |
| port.device_owner | Body | String | 포트를 사용하는 리소스 종류 |
| port.mac_address | Body | String | 포트의 MAC 주소 |
| port.port_security_enabled | Body | Boolean | 포트의 보안 상태<br>활성화된 경우 보안 그룹 적용 가능 |
| port.fixed_ips | Body | Array | 포트의 고정 IP 목록 |
| port.fixed_ips.subnet_id | Body | UUID | 포트의 고정 IP의 서브넷 ID |
| port.fixed_ips.ip_address | Body | String | 포트의 고정 IP 주소 |
| port.id | Body | UUID | 포트의 ID |
| port.security_groups | Body | Array | 포트의 보안 그룹 ID 목록 |
| port.device_id | Body | UUID | 포트를 사용하는 리소스 ID |

<details><summary>예시</summary>
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

### 포트 삭제 하기
지정한 포트를 삭제합니다.
```
DELETE /v2.0/ports/{portId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| portId | URL | UUID | O | 포트 ID |
| tokenId | Header | String | O | 토큰 ID |

#### 응답
이 API는 응답 본문을 반환하지 않습니다.

---

## 플로팅 IP
### 플로팅 IP 목록 보기
플로팅 IP 목록을 반환합니다.
```
GET /v2.0/floatingips
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| id | Query | UUID | - | 조회할 플로팅 IP ID |
| status | Query | Enum | - | 조회할 플로팅 IP의 상태<br>**ACTIVE**: 인스턴스에 연결<br>**DOWN**: 인스턴스에 미연결<br>**ERROR**: 인스턴스에 연결 또는 할당 실패 |
| tenant_id | Query | String | - | 조회할 플로팅 IP의 테넌트 ID |
| floating_network_id  | Query | UUID | - | 조회할 플로팅 IP가 속한 외부 네트워크 ID |
| fixed_ip_address | Query | String | - | 조회할 플로팅 IP가 연결된 고정 IP 주소 |
| floating_ip_address | Query | String | - | 조회할 플로팅 IP 주소 |
| port_id | Query | UUID | - | 조회할 플로팅 IP가 연결된 포트 ID |
| sort_dir | Query | Enum | - | 조회할 플로팅 IP의 정렬 방향<br>`sort_key`에서 지정한 필드를 기준으로 정렬<br>**asc**, **desc** 중 하나 |
| sort_key | Query | String | - | 조회할 플로팅 IP의 정렬 키<br>`sort_dir`에서 지정한 방향대로 정렬 |
| fields | Query | String | - | 조회할 플로팅 IP의 필드 이름<br>예) `fields=id&fields=name` |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| floatingips | Body | Array | 플로팅 IP 정보 객체 목록 |
| floatingips.floating_network_id | Body | UUID | 플로팅 IP가 속한 외부 네트워크 ID |
| floatingips.router_id | Body | UUID | 플로팅 IP가 연결된 라우터 ID |
| floatingips.fixed_ip_address | Body | String | 플로팅 IP가 연결된 고정 IP 주소 |
| floatingips.floating_ip_address | Body | String | 플로팅 IP 주소|
| floatingips.tenant_id | Body | String | 테넌트 ID |
| floatingips.status | Body | Enum | 플로팅 IP의 상태<br>**ACTIVE**: 인스턴스에 연결<br>**DOWN**: 인스턴스에 미연결<br>**ERROR**: 인스턴스에 연결 또는 할당 실패 |
| floatingips.port_id | Body | UUID | 플로팅 IP가 연결된 포트 ID |
| floatingips.id | Body | UUID | 플로팅 IP ID |

<details><summary>예시</summary>
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

### 플로팅 IP 보기
지정한 플로팅 IP에 대한 정보를 반환합니다.
```
GET /v2.0/floatingips/{floatingIpId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| floatingIpId | URL | UUID | O | 플로팅 IP ID |
| tokenId | Header | String | O | 토큰 ID |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| floatingip | Body | Object | 플로팅 IP 정보 객체 |
| floatingip.floating_network_id | Body | UUID | 플로팅 IP가 속한 외부 네트워크 ID |
| floatingip.router_id | Body | UUID | 플로팅 IP가 연결된 라우터 ID |
| floatingip.fixed_ip_address | Body | String | 플로팅 IP가 연결된 고정 IP 주소 |
| floatingip.floating_ip_address | Body | String | 플로팅 IP 주소 |
| floatingip.tenant_id | Body | String | 테넌트 ID |
| floatingip.status | Body | Enum | 플로팅 IP의 상태<br>**ACTIVE**: 인스턴스에 연결<br>**DOWN**: 인스턴스에 미연결<br>**ERROR**: 인스턴스에 연결 또는 할당 실패 |
| floatingip.port_id | Body | UUID | 플로팅 IP가 연결된 포트 ID |
| floatingip.id | Body | UUID | 플로팅 IP ID |

<details><summary>예시</summary>
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

### 플로팅 IP 생성하기
플로팅 IP를 생성합니다.
```
POST /v2.0/floatingips
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| floatingip | Body | Object | O | 플로팅 IP 생성 요청 객체 |
| floatingip.floating_network_id | Body | UUID | O | 플로팅 IP가 속한 외부 네트워크 ID |
| floatingip.port_id | Body | UUID | - | 플로팅 IP가 연결될 포트 ID |

<details><summary>예시</summary>
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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| floatingip | Body | Object | 플로팅 IP 정보 객체 |
| floatingip.floating_network_id | Body | UUID | 플로팅 IP가 속한 외부 네트워크 ID |
| floatingip.router_id | Body | UUID | 플로팅 IP가 연결된 라우터 ID |
| floatingip.fixed_ip_address | Body | String | 플로팅 IP가 연결된 고정 IP 주소 |
| floatingip.floating_ip_address | Body | String | 플로팅 IP 주소 |
| floatingip.tenant_id | Body | String | 테넌트 ID |
| floatingip.status | Body | Enum | 플로팅 IP의 상태<br>**ACTIVE**: 인스턴스에 연결<br>**DOWN**: 인스턴스에 미연결<br>**ERROR**: 인스턴스에 연결 또는 할당 실패 |
| floatingip.port_id | Body | UUID | 플로팅 IP가 연결된 포트 ID |
| floatingip.id | Body | UUID | 플로팅 IP ID |

<details><summary>예시</summary>
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

### 플로팅 IP 연결/해제하기
```
PUT /v2.0/floatingips/{floatingIpId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| floatingIpId | URL | UUID | 플로팅 IP ID |
| tokenId | Header | String | O | 토큰 ID |
| floatingip | Body | Object | O | 플로팅 IP 수정 요청 객체 |
| floatingip.port_id | Body | UUID | O | 플로팅 IP를 연결할 포트 ID<br>해제하려면 `null`을 입력 |
| floatingip.fixed_ip_address | Body | String | - | 고정 IP 주소<br>연결 또는 해제할 포트에 여러 IP가 할당된 경우, 특정 IP를 지정하기 위해 사용 |

<details><summary>예시</summary>
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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| floatingip | Body | Object | 플로팅 IP 정보 객체 |
| floatingip.floating_network_id | Body | UUID | 플로팅 IP가 속한 외부 네트워크 ID |
| floatingip.router_id | Body | UUID | 플로팅 IP가 연결된 라우터 ID |
| floatingip.fixed_ip_address | Body | String | 플로팅 IP가 연결된 고정 IP 주소 |
| floatingip.floating_ip_address | Body | String | 플로팅 IP 주소 |
| floatingip.tenant_id | Body | String | 테넌트 ID |
| floatingip.status | Body | Enum | 플로팅 IP의 상태 |
| floatingip.port_id | Body | UUID | 플로팅 IP가 연결된 포트 ID |
| floatingip.id | Body | UUID | 플로팅 IP ID |

<details><summary>예시</summary>
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

### 플로팅 IP 삭제하기
지정한 플로팅 IP를 삭제합니다.
```
DELETE /v2.0/floatingips/{floatingIpId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| floatingIpId | URL | UUID | O | 플로팅 IP ID |
| tokenId | Header | String | O | 토큰 ID |

#### 응답
이 API는 응답 본문을 반환하지 않습니다.

---

## 보안 그룹
### 보안 그룹 목록 보기
```
GET /v2.0/security-groups
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| id | Query | UUID | - | 조회할 보안 그룹 ID |
| tenant_id | Query | String | - | 조회할 보안 그룹의 테넌트 ID |
| name | Query | String | - | 조회할 보안 그룹의 이름 |
| sort_dir | Query | Enum | - | 조회할 보안 그룹의 정렬 방향<br>`sort_key`에서 지정한 필드를 기준으로 정렬<br>**asc**, **desc** 중 하나 |
| sort_key | Query | String | - | 조회할 보안 그룹의 정렬 키<br>`sort_dir`에서 지정한 방향대로 정렬 |
| fields | Query | String | - | 조회할 보안 그룹의 필드 이름<br>예) `fields=id&fields=name` |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| security_groups | Body | Array | 보안 그룹 목록 객체 |
| security_groups.tenant_id | Body | String | 테넌트 ID |
| security_groups.description | Body | String | 보안 그룹 설명 |
| security_groups.id | Body | UUID | 보안 그룹 ID |
| security_groups.security_group_rules | Body | Array | 보안 그룹 규칙 목록 |
| security_groups.name | Body | String | 보안 그룹 이름 |

<details><summary>예시</summary>
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

### 보안 그룹 보기
```
GET /v2.0/security-groups/{securityGroupId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 예시 | 필수 | 설명 |
|---|---|---|---|---|
| securityGroupId | Query | UUID | O | 조회할 보안 그룹 ID |
| tokenId | Header | String | O | 토큰 ID |
| fields | Query | String | - | 조회할 보안 그룹의 필드 이름<br>지정한 필드만 응답에 반환<br>예) `fields=id&fields=name` |

#### 응답

| 이름 | 종류 | 예시 | 설명 |
|---|---|---|---|
| security_group | Body | Object | 보안 그룹 객체 |
| security_group.tenant_id | Body | String | 테넌트 ID |
| security_group.description | Body | String | 보안 그룹 설명 |
| security_group.id | Body | UUID | 보안 그룹 ID |
| security_group.security_group_rules | Body | Array | 보안 그룹 규칙 목록 |
| security_group.name | Body | String | 보안 그룹 이름 |

<details><summary>예시</summary>
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

### 보안 그룹 생성하기

새로운 보안 그룹을 생성합니다. 새로 생성된 보안 그룹은 나가는 방향의 보안 그룹 규칙을 기본적으로 포함합니다.

```
POST /v2.0/security-groups
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| security_group | Body | Object | O | 보안 그룹 생성 요청 객체 |
| description | Body | String | - | 보안 그룹 설명 |
| name | Body | String | - | 보안 그룹 이름 |

<details><summary>예시</summary>
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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| security_group | Body | Object | 보안 그룹 객체 |
| security_group.tenant_id | Body | String | 테넌트 ID |
| security_group.description | Body | String | 보안 그룹 설명 |
| security_group.id | Body | UUID | 보안 그룹 ID |
| security_group.security_group_rules | Body | Array | 보안 그룹 규칙 목록 |
| security_group.name | Body | String | 보안 그룹 이름 |

<details><summary>예시</summary>
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

### 보안 그룹 수정하기
기존 보안 그룹을 수정합니다.
```
PUT /v2.0/security-groups/{securityGroupId}
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| securityGroupId | URL | UUID | O | 보안 그룹 ID |
| security_group | Body | Object | O | 보안 그룹 수정 요청 객체 |
| description | Body | String | - | 보안 그룹 설명 |
| name | Body | String | - | 보안 그룹 이름 |

<details><summary>예시</summary>
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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| security_group | Body | Object | 보안 그룹 객체 |
| security_group.tenant_id | Body | String | 테넌트 ID |
| security_group.description | Body | String | 보안 그룹 설명 |
| security_group.id | Body | UUID | 보안 그룹 ID |
| security_group.security_group_rules | Body | Array | 보안 그룹 규칙 목록 |
| security_group.name | Body | String | 보안 그룹 이름 |

<details><summary>예시</summary>
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

### 보안 그룹 삭제하기
지정한 보안 그룹을 삭제합니다.
```
DELETE /v2.0/security-groups/{securityGroupId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| securityGroupId | URL | UUID | O | 보안 그룹 ID |
| tokenId | Header | String | O | 토큰 ID |

#### 응답
이 API는 응답 본문을 반환하지 않습니다.

---

## 보안 규칙
### 보안 규칙 목록 보기
```
GET /v2.0/security-group-rules
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| id | Query | UUID | - | 조회할 보안 규칙 ID |
| remote_group_id | Query | UUID | - | 조회할 보안 규칙의 원격 보안 그룹 ID |
| protocol | Query | String | - | 조회할 보안 규칙의 프로토콜 |
| direction | Query | Enum | - | 조회할 보안 규칙이 적용되는 패킷 방향<br>**ingress** 또는 **egress** |
| ethertype | Query | Enum | - | 조회할 보안 규칙의 네트워크 트래픽 `Ethertype` 값<br>**IPv4** 또는 **IPv6** |
| port_range_max | Query | Integer | - | 조회할 보안 규칙의 포트 범위 최댓값 |
| port_range_min | Query | Integer | - | 조회할 보안 규칙의 포트 범위 최솟값 |
| security_group_id | Query | UUID | - | 조회할 보안 규칙이 속한 보안 그룹 ID |
| tenant_id | Query | String | - | 조회할 보안 규칙의 테넌트 ID |
| remote_ip_prefix | Query | String | - | 조회할 보안 규칙의 목적지 IP 접두사 |
| description | Query | String | - | 조회할 보안 규칙의 설명 |
| sort_dir | Query | Enum | - | 조회할 보안 규칙의 정렬 방향<br>`sort_key`에서 지정한 필드를 기준으로 정렬<br>**asc**, **desc** 중 하나 |
| sort_key | Query | String | - | 조회할 보안 규칙의 정렬 키<br>`sort_dir`에서 지정한 방향대로 정렬 |
| fields | Query | String | - | 조회할 보안 규칙의 필드 이름<br>예) `fields=id&fields=name` |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| security_group_rules | Body | Array | 보안 규칙 객체 목록 |
| security_group_rules.direction | Body | Enum | 보안 규칙이 적용되는 패킷 방향<br>**ingress** 또는 **egress** |
| security_group_rules.ethertype | Body | Enum | 보안 규칙의 네트워크 트래픽 `Ethertype` 값<br>**IPv4** 또는 **IPv6** |
| security_group_rules.protocol | Body | String | 보안 규칙의 프로토콜 이름 |
| security_group_rules.description | Body | String | 보안 규칙 설명 |
| security_group_rules.port_range_max | Body | Integer | 보안 규칙의 포트 범위 최댓값 |
| security_group_rules.port_range_min | Body | Integer | 보안 규칙의 포트 범위 최솟값 |
| security_group_rules.remote_group_id | Body | UUID | 보안 규칙의 원격 보안 그룹 ID |
| security_group_rules.remote_ip_prefix | Body | Enum | 보안 규칙의 목적지 IP 접두사 |
| security_group_rules.security_group_id | Body | UUID | 보안 규칙이 속한 보안 그룹 ID |
| security_group_rules.tenant_id | Body | String | 테넌트 ID |
| security_group_rules.id | Body | UUID | 보안 규칙 ID |

<details><summary>예시</summary>
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

### 보안 규칙 보기
```
GET /v2.0/security-group-rules/{securityGroupRuleId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| securityGroupRuleId | URL | UUID | O | 보안 규칙 ID |
| tokenId | Header | String | O | 토큰 ID |
| fields | Query | String | - | 조회할 보안 규칙의 필드 이름<br>예) `fields=id&fields=name` |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| security_group_rule | Body | Object | 보안 규칙 객체 |
| security_group_rule.direction | Body | Enum | 보안 규칙이 적용되는 패킷 방향<br>**ingress** 또는 **egress** |
| security_group_rule.ethertype | Body | Enum | 보안 규칙의 네트워크 트래픽 `Ethertype` 값<br>**IPv4** 또는 **IPv6** |
| security_group_rule.protocol | Body | String | 보안 규칙의 프로토콜 이름 |
| security_group_rule.description | Body | String | 보안 규칙 설명 |
| security_group_rule.port_range_max | Body | Integer | 조회할 보안 규칙의 포트 범위 최댓값 |
| security_group_rule.port_range_min | Body | Integer | 조회할 보안 규칙의 포트 범위 최솟값 |
| security_group_rule.remote_group_id | Body | UUID | 보안 규칙의 원격 보안 그룹 ID |
| security_group_rule.remote_ip_prefix | Body | Enum | 보안 규칙의 목적지 IP 접두사 |
| security_group_rule.security_group_id | Body | UUID | 보안 규칙이 속한 보안 그룹 ID |
| security_group_rule.tenant_id | Body | String | 테넌트 ID |
| security_group_rule.id | Body | UUID | 보안 규칙 ID |

<details><summary>예시</summary>
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

### 보안 규칙 생성하기

새로운 보안 그룹 규칙을 생성합니다. IPv4에 대한 보안 규칙만 생성할 수 있습니다.

```
POST /v2.0/security-group-rules
X-Auth-Token: {tokenId}
```

#### 요청

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| security_group_rule | Body | Object | O | 보안 규칙 생성 요청 객체 |
| security_group_rule.remote_group_id | Body | UUID | - | 보안 규칙의 원격 보안 그룹 ID |
| security_group_rule.direction | Body | Enum | O | 보안 규칙이 적용되는 패킷 방향<br>**ingress**, **egress** |
| security_group_rule.ethertype | Body | Enum | - | `IPv4`로 지정. 생략 시에 `IPv4`로 지정 |
| security_group_rule.protocol | Body | String | - | 보안 규칙의 프로토콜 이름. 생략 시에 모든 프로토콜에 적용. |
| security_group_rule.port_range_max | Body | Integer | - | 보안 규칙의 포트 범위 최댓값 |
| security_group_rule.port_range_min | Body | Integer | - | 보안 규칙의 포트 범위 최솟값 |
| security_group_rule.security_group_id | Body | UUID | O | 보안 규칙이 속한 보안 그룹 ID |
| security_group_rule.remote_ip_prefix | Body | Enum | - | 보안 규칙의 목적지 IP 접두사 |
| security_group_rule.description | Body | String | - | 보안 규칙 설명 |

<details><summary>예시</summary>
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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| security_group_rule | Body | Object | 보안 규칙 객체 |
| security_group_rule.direction | Body | Enum | 보안 규칙이 적용되는 패킷 방향<br>**ingress** 또는 **egress** |
| security_group_rule.ethertype | Body | Enum | 보안 규칙의 네트워크 트래픽 `Ethertype` 값<br>**IPv4** 또는 **IPv6** |
| security_group_rule.protocol | Body | String | 보안 규칙의 프로토콜 이름 |
| security_group_rule.description | Body | String | 보안 규칙 설명 |
| security_group_rule.port_range_max | Body | Integer | 조회할 보안 규칙의 포트 범위 최댓값 |
| security_group_rule.port_range_min | Body | Integer | 조회할 보안 규칙의 포트 범위 최솟값 |
| security_group_rule.remote_group_id | Body | UUID | 보안 규칙의 원격 보안 그룹 ID |
| security_group_rule.remote_ip_prefix | Body | Enum | 보안 규칙의 목적지 IP 접두사 |
| security_group_rule.security_group_id | Body | UUID | 보안 규칙이 속한 보안 그룹 ID |
| security_group_rule.tenant_id | Body | String | 테넌트 ID |
| security_group_rule.id | Body | UUID | 보안 규칙 ID |

<details><summary>예시</summary>
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

### 보안 규칙 삭제하기
지정한 보안 규칙을 삭제합니다.
```
DELETE /v2.0/security-group-rules/{securityGroupRuleId}
X-Auth-Token: {tokenId}
```

#### 요청
이 API는 요청 본문을 요구하지 않습니다.

| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| securityGroupRuleId | URL | UUID | O | 보안 규칙 ID |
| tokenId | Header | String | O | 토큰 ID |

#### 응답
이 API는 응답 본문을 반환하지 않습니다.

---

