## Network > VPC > OpenStack 호환 API 가이드

NHN Cloud Network 서비스는 OpenStack neutron API와 호환되는 API를 제공합니다. 
제공하는 OpenStack 호환 API는 다음과 같습니다.

| OpenStack API 메서드 | 용도 |
| --- | --- |
| GET networks | 네트워크 목록 보기 |
| GET subnets | 서브넷 목록 보기 |
| GET ports | 포트 목록 보기 |
| POST port | 포트 생성하기 |
| PUT port | 포트 변경하기 |
| DELETE port | 포트 삭제하기 |

OpenStack 호환 API의 요청 및 응답에 포함되는 필드는 OpenStack neutron API가 제공하는 항목 중 NHN Cloud의 정책에 따라 본 문서에 명시된 항목으로 제한됩니다.

API를 사용하려면 API 엔드포인트와 토큰 등이 필요합니다. [API 사용 준비](/Compute/Compute/ko/identity-api-ncgn/)를 참고하여 API 사용에 필요한 정보를 준비합니다.

OpenStack 호환 API는 `network` 타입 엔드포인트를 이용합니다. 정확한 엔드포인트는 토큰 발급 응답의 `serviceCatalog`를 참조합니다.

| 타입 | 리전 | 엔드포인트 |
|---|---|---|
| network | 한국(판교) 리전 | https://kr1-api-network-infrastructure.gncloud.go.kr  |


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
| networks.status | Body | Enum | 네트워크 상태<br>`ACTIVE`, `BUILD`, `DOWN` 중 하나. |
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
| id | Query | UUID | - | 조회할 포트 ID |
| status | Query | Enum | - | 조회할 포트 상태<br>`ACTIVE`, `BUILD`, `DOWN` 중 하나. |
| name | Query | String | - | 조회할 포트 이름 |
| admin_state | Query | Boolean | - | 조회할 포트의 관리자 제어 상태 |
| network_id | Query | UUID | - | 조회할 포트의 네트워크 ID |
| tenant_id | Query | String | - | 조회할 포트의 테넌트 ID |
| fixed_ips | Query | array | - | 조회할 포트의 고정 IP 정보.<br>IP 주소(`ip_address`), IP 주소 일부(`ip_address_substr`), 서브넷 ID(`subnet_id`)로 필터링 가능 | 
| mac_address | Query | String | - | 조회할 포트의 MAC 주소 |
| device_owner | Query | String | - | 조회할 포트를 사용하는 리소스 종류 |
| device_id | Query | UUID | - | 조회할 포트를 사용하는 리소스 ID |
| fields | Query | String | - | 조회할 포트의 필드 이름<br>예) `fields=id&fields=name` |

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| ports | Body | Array | 포트 정보 객체 목록 |
| ports.id | Body | UUID | 포트의 ID |
| ports.name | Body | String | 포트 이름 |
| ports.status | Body | Enum | 포트 상태<br>`ACTIVE`, `BUILD`, `DOWN` 중 하나. |
| ports.admin_state_up | Body | Boolean | 포트의 관리자 제어 상태 |
| ports.network_id | Body | UUID | 포트의 네트워크 ID |
| ports.tenant_id | Body | String | 테넌트 ID |
| ports.extra_dhcp_opts | Body | Array | 추가 DHCP 옵션 |
| ports.binding:vnic_type | Body | String | 포트에 연결된 vNIC 타입 |
| ports.device_owner | Body | String | 포트를 사용하는 리소스 종류 |
| ports.device_id | Body | UUID | 포트를 사용하는 리소스 ID |
| ports.mac_address | Body | String | 포트의 MAC 주소 |
| ports.fixed_ips | Body | Array | 포트의 고정 IP 목록 |
| ports.fixed_ips.subnet_id | Body | UUID | 고정 IP가 속한 서브넷 ID |
| ports.fixed_ips.ip_address | Body | String | 고정 IP 주소 |
| ports.port_security_enabled | Body | Boolean | 포트의 보안 상태<br>활성화된 경우 보안 그룹, 허용 주소 쌍 설정 가능 |
| ports.security_groups | Body | Array | 포트에 설정된 보안 그룹 ID 목록 |
| ports.allowed_address_pairs | Body | Array | 포트에 설정된 허용 주소 쌍 목록 |

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
| port | Body | Array | 포트 정보 객체 |
| port.id | Body | UUID | 포트의 ID |
| port.name | Body | String | 포트 이름 |
| port.status | Body | Enum | 포트 상태<br>`ACTIVE`, `BUILD`, `DOWN` 중 하나. |
| port.admin_state_up | Body | Boolean | 포트의 관리자 제어 상태 |
| port.network_id | Body | UUID | 포트의 네트워크 ID |
| port.tenant_id | Body | String | 테넌트 ID |
| port.extra_dhcp_opts | Body | Array | 추가 DHCP 옵션 |
| port.binding:vnic_type | Body | String | 포트에 연결된 vNIC 타입 |
| port.device_owner | Body | String | 포트를 사용하는 리소스 종류 |
| port.device_id | Body | UUID | 포트를 사용하는 리소스 ID |
| port.mac_address | Body | String | 포트의 MAC 주소 |
| port.fixed_ips | Body | Array | 포트의 고정 IP 목록 |
| port.fixed_ips.subnet_id | Body | UUID | 고정 IP가 속한 서브넷 ID |
| port.fixed_ips.ip_address | Body | String | 고정 IP 주소 |
| port.port_security_enabled | Body | Boolean | 포트의 보안 상태<br>활성화된 경우 보안 그룹, 허용 주소 쌍 설정 가능 |
| port.security_groups | Body | Array | 포트에 설정된 보안 그룹 ID 목록 |
| port.allowed_address_pairs | Body | Array | 포트에 설정된 허용 주소 쌍 목록 |


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

### 포트 생성하기
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
| port.admin_state_up | Body | Boolean | - | 포트의 관리자 제어 상태. 기본값 `true` |
| port.fixed_ips | Body | Array | - | 포트의 고정 IP 목록 |
| port.fixed_ips.subnet_id | Body | UUID | - | 고정 IP를 할당할 서브넷 ID |
| port.fixed_ips.ip_address | Body | String | - | 고정 IP 주소 |
| port.port_security_enabled | Body | Boolean | - | 포트 보안 사용 여부. 기본값 `true` | 
| port.security_groups | Body | Array | - | 포트에 설정할 보안 그룹 ID 목록. 기본값 `default 보안 그룹의 ID`<br>포트 보안 사용 시 설정 가능 |
| port.allowed_address_pairs | Body | Array | - | 포트의 허용 주소 쌍 목록<br>포트 보안 사용 시 설정 가능 |
| port.allowed_address_pairs.ip_address | Body | String | - | 허용할 IP 주소 |
| port.allowed_address_pairs.mac_address | Body | String | - | 허용할 MAC 주소 |
| port.extra_dhcp_opts | Body | Array | - | 추가 DHCP 옵션 |
| port.device_owner | Body | String | - | 포트를 사용하는 리소스 종류 |
| port.device_id | Body | UUID | - | 포트를 사용하는 리소스 ID. 가상 IP로 사용할 경우 `network:virtual_ip`로 지정 |

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
| port.id | Body | UUID | 포트의 ID |
| port.name | Body | String | 포트 이름 |
| port.status | Body | Enum | 포트 상태<br>`ACTIVE`, `BUILD`, `DOWN` 중 하나. |
| port.admin_state_up | Body | Boolean | 포트의 관리자 제어 상태 |
| port.network_id | Body | UUID | 포트의 네트워크 ID |
| port.tenant_id | Body | String | 테넌트 ID |
| port.extra_dhcp_opts | Body | Array | 추가 DHCP 옵션 |
| port.binding:vnic_type | Body | String | 포트에 연결된 vNIC 타입 |
| port.device_owner | Body | String | 포트를 사용하는 리소스 종류 |
| port.device_id | Body | UUID | 포트를 사용하는 리소스 ID |
| port.mac_address | Body | String | 포트의 MAC 주소 |
| port.fixed_ips | Body | Array | 포트의 고정 IP 목록 |
| port.fixed_ips.subnet_id | Body | UUID | 고정 IP가 속한 서브넷 ID |
| port.fixed_ips.ip_address | Body | String | 고정 IP 주소 |
| port.port_security_enabled | Body | Boolean | 포트의 보안 상태<br>활성화된 경우 보안 그룹, 허용 주소 쌍 설정 가능 |
| port.security_groups | Body | Array | 포트에 설정된 보안 그룹 ID 목록 |
| port.allowed_address_pairs | Body | Array | 포트에 설정된 허용 주소 쌍 목록 |

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

### 포트 변경하기
지정한 포트의 속성을 변경합니다.
```
PUT /v2.0/ports/{portId}
X-Auth-Token: {tokenId}
```

#### 요청
| 이름 | 종류 | 형식 | 필수 | 설명 |
|---|---|---|---|---|
| tokenId | Header | String | O | 토큰 ID |
| port | Body | Object | O | 포트 생성 요청 객체 |
| port.name | Body | String | - | 포트 이름 |
| port.admin_state_up | Body | Boolean | - | 포트의 관리자 제어 상태 |
| port.fixed_ips | Body | Array | - | 포트의 고정 IP 목록 |
| port.fixed_ips.subnet_id | Body | UUID | - | 고정 IP를 할당할 서브넷 ID |
| port.fixed_ips.ip_address | Body | String | - | 고정 IP 주소 |
| port.port_security_enabled | Body | Boolean | - | 포트 보안 사용 여부.<br>보안 그룹 및 허용 주소 쌍이 모두 제거된 이후 `false`로 변경 가능 | 
| port.security_groups | Body | Array | - | 포트에 설정할 보안 그룹 ID 목록. 빈 목록 입력 시 전체 제거<br>포트 보안 사용 시 설정 가능 |
| port.allowed_address_pairs | Body | Array | - | 포트의 허용 주소 쌍 목록. 빈 목록 입력 시 전체 제거<br>포트 보안 사용 시 설정 가능 |
| port.allowed_address_pairs.ip_address | Body | String | - | 허용할 IP 주소 |
| port.allowed_address_pairs.mac_address | Body | String | - | 허용할 MAC 주소 |
| port.extra_dhcp_opts | Body | Array | - | 추가 DHCP 옵션 |

<details><summary>예시</summary>
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

#### 응답

| 이름 | 종류 | 형식 | 설명 |
|---|---|---|---|
| port | Body | Array | 포트 정보 객체 |
| port.id | Body | UUID | 포트의 ID |
| port.name | Body | String | 포트 이름 |
| port.status | Body | Enum | 포트 상태<br>`ACTIVE`, `BUILD`, `DOWN` 중 하나. |
| port.admin_state_up | Body | Boolean | 포트의 관리자 제어 상태 |
| port.network_id | Body | UUID | 포트의 네트워크 ID |
| port.tenant_id | Body | String | 테넌트 ID |
| port.extra_dhcp_opts | Body | Array | 추가 DHCP 옵션 |
| port.binding:vnic_type | Body | String | 포트에 연결된  타입 |
| port.device_owner | Body | String | 포트를 사용하는 리소스 종류 |
| port.device_id | Body | UUID | 포트를 사용하는 리소스 ID |
| port.mac_address | Body | String | 포트의 MAC 주소 |
| port.fixed_ips | Body | Array | 포트의 고정 IP 목록 |
| port.fixed_ips.subnet_id | Body | UUID | 고정 IP가 속한 서브넷 ID |
| port.fixed_ips.ip_address | Body | String | 고정 IP 주소 |
| port.port_security_enabled | Body | Boolean | 포트의 보안 상태<br>활성화된 경우 보안 그룹, 허용 주소 쌍 설정 가능 |
| port.security_groups | Body | Array | 포트에 설정된 보안 그룹 ID 목록 |
| port.allowed_address_pairs | Body | Array | 포트에 설정된 허용 주소 쌍 목록 |


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

### 포트 삭제하기
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
