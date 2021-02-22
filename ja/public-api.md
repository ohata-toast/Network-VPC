## Network > VPC > API v2ガイド

APIを使用するにはAPIエンドポイントとトークンなどが必要です。[API使用準備](/Compute/Compute/ko/identity-api/)を参照してAPIを使用するのに必要な情報を準備します。

VPC APIは`network`タイプエンドポイントを利用します。正確なエンドポイントはトークン発行レスポンスの`serviceCatalog`を参照します。

| タイプ | リージョン | エンドポイント |
|---|---|---|
| network | 韓国(パンギョ)リージョン<br>韓国(坪村)リージョン<br>日本リージョン | https://kr1-api-network.infrastructure.cloud.toast.com<br>https://kr2-api-network.infrastructure.cloud.toast.com<br>https://jp1-api-network.infrastructure.cloud.toast.com |

APIレスポンスにガイドに明示されていないフィールドが表示される場合があります。それらのフィールドは、TOAST内部用途で使用され、事前に告知せずに変更する場合があるため使用しないでください。

## ネットワーク
### ネットワークリスト表示
使用可能なネットワークリストを返します。
```
GET /v2.0/networks
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| id | Query | UUID | - | 照会するネットワークID |
| name | Query | String | - | 照会するネットワーク名 |
| provider:network_type | Query | Enum | - | 照会するネットワークタイプ<br>`flat`、`vlan`のどちらか |
| router:external | Query | Boolean | - | 照会するネットワークの外部接続有無 |
| shared | Query | Boolean | - | 照会するネットワークの共有有無 |
| tenant_id | Query | String | - | 照会するネットワークが属しているテナントID |
| sort_dir | Query | Enum | - | 照会するネットワークのソート方向<br>`sort_key`で指定したフィールドを基準にソート<br>**asc**、**desc**のどちらか |
| sort_key | Query | String | - | 照会するネットワークのソートキー<br>`sort_dir`で指定した方向通りにソート |
| fields | Query | String | - | 照会するネットワークのフィールド名<br>例) `fields=id&fields=name` |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| networks | Body | Array | ネットワーク情報オブジェクトリスト |
| networks.status | Body | Enum | ネットワークの状態<br>**ACTIVE**、**DOWN**、**BUILD**、**ERROR**のいずれか。 |
| networks.subnets | Body | Array | ネットワークに属しているサブネットのIDリスト |
| networks.name | Body | String | ネットワーク名 |
| networks.router:external | Body | Boolean | ネットワーク外部接続有無 |
| networks.tenant_id | Body | String | テナントID |
| networks.admin_state_up | Body | Boolean | 管理者制御状態<br>`true`：使用可能<br>`false`：使用不可 |
| networks.mtu | Body | Integer | 最大転送単位(Maximum Transmission Unit) |
| networks.shared | Body | Boolean | ネットワーク共有有無 |
| networks.port_security_enabled | Body | Boolean | ネットワークポートのセキュリティ有無<br>このネットワークで作成されるポートのセキュリティ有無を決定 |
| networks.id | Body | String | ネットワークID |
| networks.name | Body | String | ネットワーク名 |
| networks_links | Body | Array | ページネーション用の情報オブジェクト<br>`limit`、`offset`を追加した場合に返す<br>次のリストを指すパスを含む |

<details><summary>例</summary>
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

## サブネット
### サブネットリスト表示
使用可能なサブネットリストを返します。
```
GET /v2.0/subnets
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| id | Query | UUID | - | 照会するサブネットID |
| name | Query | String | - | 照会するサブネット名 |
| enable_dhcp | Query | Boolean | - | 照会するサブネットのDHCP有無 |
| network_id | Query | UUID | - | 照会するサブネットのネットワークID |
| cidr | Query | String | - | 照会するサブネットのCIDR |
| shared | Query | Boolean | - | 照会するサブネットの共有有無 |
| sort_dir | Query | Enum | - | 照会するサブネットのソート方向<br>`sort_key`で指定したフィールドを基準にソート<br>**asc**、**desc**のいずれか |
| sort_key | Query | String | - | 照会するサブネットのソートキー<br>`sort_dir`で指定した方向通りにソート |
| fields | Query | String | - | 照会するサブネットのフィールド名<br>例) `fields=id&fields=name` |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| subnets | Body | Array | サブネット情報オブジェクトリスト |
| subnets.name | Body | String | サブネット名 |
| subnets.enable_dhcp | Body | Boolean | サブネットのDHCP有無 |
| subnets.network_id | Body | UUID | サブネットのネットワークID |
| subnets.tenant_id | Body | String | テナントID |
| subnets.dns_nameservers | Body | Array | サブネットに接続されたDNSネームサーバーリスト |
| subnets.gateway_ip | Body | String | サブネットのゲートウェイIP |
| subnets.ipv6_ra_mode | Body | Boolean | IPv6のRouter Advertisementモード |
| subnets.allocation_pools | Body | Array | サブネットIP範囲オブジェクトリスト |
| subnets.allocation_pools.start | Body | String | サブネットIP範囲の最初のIPアドレス |
| subnets.allocation_pools.end | Body | String | サブネットIP範囲の最後のIPアドレス |
| subnets.host_routes | Body | Array | サブネットの追加パス情報リスト |
| subnets.host_routes.destination | Body | String | 宛先<br>宛先アドレスが`destination`の場合<br>`nexthop`に指定されたアドレスへ伝達 |
| subnets.host_routes.nexthop | Body | String | 次のhopアドレス |
| subnets.ip_version | Body | Integer | IPプロトコルバージョン<br>4または6 |
| subnets.ipv6_address_mode | Body | String | IPv6のアドレス割り当てモード |
| subnets.cidr | Body | String | サブネットのCIDR |
| subnets.id | Body | UUID | サブネットのID |
| subnets.subnetpool_id | Body | UUID | サブネットPool ID |
| subnets_links | Body | Array | ページネーション用の情報オブジェクト<br>`limit`、`offset`を追加した場合に返す<br>次のリストを指すパスを含む |

<details><summary>例</summary>
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

## ポート
### ポートリスト表示
ポートリストを返します。
```
GET /v2.0/ports
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| id | Query | UUID | - | 照会するポートIP ID |
| status | Query | Enum | - | 照会するポート状態<br>**ACTIVE**、**DOWN**のいずれか。 |
| display_name | Query | UUID | - | 照会するポート名 |
| admin_state | Query | Boolean | - | 照会するポートの管理者制御状態 |
| network_id | Query | UUID | - | 照会するポートのネットワークID |
| tenant_id | Query | String | - | 照会するポートのテナントID |
| device_owner | Query | String | - | 照会するポートを使用するリソースの種類 |
| mac_address | Query | String | - | 照会するポートのMACアドレス |
| port_id | Query | UUID | - | 照会するポートのID |
| security_groups | Query | UUID | - | 照会するポートのセキュリティグループID |
| device_id | Query | UUID | - | 照会するポートを使用するリソースID |
| fields | Query | String | - | 照会するポートのフィールド名<br>例) `fields=id&fields=name` |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| ports | Body | Array | ポート情報オブジェクトリスト |
| ports.status | Body | Enum | ポート状態<br>**ACTIVE**、**DOWN**のいずれか。 |
| ports.name | Body | String | ポート名 |
| ports.allowed_address_pairs | Body | Array | ポートのアドレスペアリスト |
| ports.admin_state_up | Body | Boolean | ポートの管理者制御状態 |
| ports.network_id | Body | UUID | ポートのネットワークID |
| ports.tenant_id | Body | String | テナントID |
| ports.extra_dhcp_opts | Body | Array | 追加DHCP設定 |
| ports.binding:vnic_type | Body | String | ポートタイプ |
| ports.device_owner | Body | String | ポートを使用するリソースの種類 |
| ports.mac_address | Body | String | ポートのMACアドレス |
| ports.port_security_enabled | Body | Boolean | ポートのセキュリティ状態<br>アクティブの場合、セキュリティグループを適用可能 |
| ports.fixed_ips | Body | Array | ポートの固定IPリスト |
| ports.fixed_ips.subnet_id | Body | UUID | ポートの固定IPのサブネットID |
| ports.fixed_ips.ip_address | Body | String | ポートの固定IPアドレス |
| ports.id | Body | UUID | ポートのID |
| ports.security_groups | Body | Array | ポートのセキュリティグループIDリスト |
| ports.device_id | Body | UUID | ポートを使用するリソースID |

<details><summary>例</summary>
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

### ポート表示

```
GET /v2.0/ports/{portId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| portId | URL | UUID | O | ポートID |
| tokenId | Header | String | O | トークンID |
| fields | Query | String | - | 照会するポートのフィールド名<br>例) `fields=id&fields=name` |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| port | Body | Object | ポート情報オブジェクト |
| port.status | Body | Enum | ポート状態<br>**ACTIVE**、**DOWN**のいずれか |
| port.name | Body | String | ポート名 |
| port.allowed_address_pairs | Body | Array | ポートのアドレスペアリスト |
| port.admin_state_up | Body | Boolean | ポートの管理者制御状態 |
| port.network_id | Body | UUID | ポートのネットワークID |
| port.tenant_id | Body | String | テナントID |
| port.extra_dhcp_opts | Body | Array | 追加DHCP設定 |
| port.binding:vnic_type | Body | String | ポートタイプ |
| port.device_owner | Body | String | ポートを使用するリソースの種類 |
| port.mac_address | Body | String | ポートのMACアドレス |
| port.port_security_enabled | Body | Boolean | ポートのセキュリティ状態<br>アクティブの場合、セキュリティグループを適用可能 |
| port.fixed_ips | Body | Array | ポートの固定IPリスト |
| port.fixed_ips.subnet_id | Body | UUID | ポートの固定IPのサブネットID |
| port.fixed_ips.ip_address | Body | String | ポートの固定IPアドレス |
| port.id | Body | UUID | ポートのID |
| port.security_groups | Body | Array | ポートのセキュリティグループIDリスト |
| port.device_id | Body | UUID | ポートを使用するリソースID |

<details><summary>例</summary>
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

### ポートを作成する
新しいポートを作成します。作成したポートはインスタンス作成時に活用できます。
```
POST /v2.0/ports
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| port | Body | Object | O | ポート作成リクエストオブジェクト |
| port.name | Body | String | - | ポート名 |
| port.network_id | Body | UUID | O | ポートのネットワークID |
| port.admin_state_up | Body | Boolean | - | ポートの管理者制御状態 |
| port.mac_address | Body | String | - | ポートのMACアドレス |
| port.port_id | Body | UUID | - | Floating IPが接続されたポートID |
| port.fixed_ips | Body | Array | - | ポートの固定IPリスト |
| port.fixed_ips.subnet_id | Body | UUID | - | ポートの固定IPのサブネットID |
| port.fixed_ips.ip_address | Body | String | - | ポートの固定IPアドレス |
| port.security_groups | Body | Array | - | ポートのセキュリティグループIDリスト |
| port.allowed_address_pairs | Body | Array | - | ポートのアドレスペアリスト |
| port.allowed_address_pairs.ip_address | Body | String | - | ポートのIPアドレス |
| port.allowed_address_pairs.mac_address | Body | String | - | ポートのMACアドレス |
| port.extra_dhcp_opts | Body | Array | - | 追加DHCP設定 |
| port.device_owner | Body | String | - | ポートを使用するリソースの種類 |
| port.device_id | Body | UUID | - | ポートを使用するリソースID |

<details><summary>例</summary>
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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| port | Body | Array | ポート情報オブジェクト |
| port.status | Body | Enum | ポートの状態<br>**ACTIVE**、**DOWN**のいずれか |
| port.name | Body | String | ポート名 |
| port.allowed_address_pairs | Body | Array | ポートのアドレスペアリスト |
| port.admin_state_up | Body | Boolean | ポートの管理者制御状態 |
| port.network_id | Body | UUID | ポートのネットワークID |
| port.tenant_id | Body | String | テナントID |
| port.extra_dhcp_opts | Body | Array | 追加DHCP設定 |
| port.binding:vnic_type | Body | String | ポートタイプ |
| port.device_owner | Body | String | ポートを使用するリソースの種類 |
| port.mac_address | Body | String | ポートのMACアドレス |
| port.port_security_enabled | Body | Boolean | ポートのセキュリティ状態<br>アクティブの場合、セキュリティグループを適用可能 |
| port.fixed_ips | Body | Array | ポートの固定IPリスト |
| port.fixed_ips.subnet_id | Body | UUID | ポートの固定IPのサブネットID |
| port.fixed_ips.ip_address | Body | String | ポートの固定IPアドレス |
| port.id | Body | UUID | ポートのID |
| port.security_groups | Body | Array | ポートのセキュリティグループIDリスト |
| port.device_id | Body | UUID | ポートを使用するリソースID |

<details><summary>例</summary>
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

### ポートを削除する
指定したポートを削除します。
```
DELETE /v2.0/ports/{portId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| portId | URL | UUID | O | ポートID |
| tokenId | Header | String | O | トークンID |

#### レスポンス
このAPIはレスポンス本文を返しません。

---

## Floating IP
### Floating IPリスト表示
Floating IPリストを返します。
```
GET /v2.0/floatingips
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| id | Query | UUID | - | 照会するFloating IP ID |
| status | Query | Enum | - | 照会するFloating IPの状態<br>**ACTIVE**：インスタンスに接続<br>**DOWN**：インスタンスに未接続<br>**ERROR**：インスタンスに接続または割り当てに失敗 |
| tenant_id | Query | String | - | 照会するFloating IPのテナントID |
| floating_network_id  | Query | UUID | - | 照会するFloating IPが属している外部ネットワークID |
| fixed_ip_address | Query | String | - | 照会するFloating IPが接続された固定IPアドレス |
| floating_ip_address | Query | String | - | 照会するFloating IPアドレス |
| port_id | Query | UUID | - | 照会するFloating IPが接続されたポートID |
| sort_dir | Query | Enum | - | 照会するFloating IPのソート方向<br>`sort_key`で指定したフィールドを基準にソート<br>**asc**、**desc**のいずれか |
| sort_key | Query | String | - | 照会するFloating IPのソートキー<br>`sort_dir`で指定した方向通りにソート |
| fields | Query | String | - | 照会するFloating IPのフィールド名<br>例) `fields=id&fields=name` |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| floatingips | Body | Array | Floating IP情報オブジェクトリスト |
| floatingips.floating_network_id | Body | UUID | Floating IPが属している外部ネットワークID |
| floatingips.router_id | Body | UUID | Floating IPが接続されたルーターID |
| floatingips.fixed_ip_address | Body | String | Floating IPが接続された固定IPアドレス |
| floatingips.floating_ip_address | Body | String | Floating IPアドレス|
| floatingips.tenant_id | Body | String | テナントID |
| floatingips.status | Body | Enum | Floating IPの状態<br>**ACTIVE**：インスタンスに接続<br>**DOWN**：インスタンスに未接続<br>**ERROR**：インスタンスに接続または割り当て失敗 |
| floatingips.port_id | Body | UUID | Floating IPが接続されたポートID |
| floatingips.id | Body | UUID | Floating IP ID |

<details><summary>例</summary>
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

### Floating IP表示
指定したFloating IPの情報を返します。
```
GET /v2.0/floatingips/{floatingIpId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| floatingIpId | URL | UUID | O | Floating IP ID |
| tokenId | Header | String | O | トークンID |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| floatingip | Body | Object | Floating IP情報オブジェクト |
| floatingip.floating_network_id | Body | UUID | Floating IPが属している外部ネットワークID |
| floatingip.router_id | Body | UUID | Floating IPが接続されたルーターID |
| floatingip.fixed_ip_address | Body | String | Floating IPが接続された固定IPアドレス |
| floatingip.floating_ip_address | Body | String | Floating IPアドレス |
| floatingip.tenant_id | Body | String | テナントID |
| floatingip.status | Body | Enum | Floating IPの状態<br>**ACTIVE**：インスタンスに接続<br>**DOWN**：インスタンスに未接続<br>**ERROR**：インスタンスに接続または割り当て失敗 |
| floatingip.port_id | Body | UUID | Floating IPが接続されたポートID |
| floatingip.id | Body | UUID | Floating IP ID |

<details><summary>例</summary>
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

### Floating IPを作成する
Floating IPを作成します。
```
POST /v2.0/floatingips
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| floatingip | Body | Object | O | Floating IP作成リクエストオブジェクト |
| floatingip.floating_network_id | Body | UUID | O | Floating IPが属している外部ネットワークID |
| floatingip.port_id | Body | UUID | - | Floating IPが接続されるポートID |

<details><summary>例</summary>
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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| floatingip | Body | Object | Floating IP情報オブジェクト |
| floatingip.floating_network_id | Body | UUID | Floating IPが属している外部ネットワークID |
| floatingip.router_id | Body | UUID | Floating IPが接続されたルーターID |
| floatingip.fixed_ip_address | Body | String | Floating IPが接続された固定IPアドレス |
| floatingip.floating_ip_address | Body | String | Floating IPアドレス |
| floatingip.tenant_id | Body | String | テナントID |
| floatingip.status | Body | Enum | Floating IPの状態<br>**ACTIVE**：インスタンスに接続<br>**DOWN**：インスタンスに未接続<br>**ERROR**：インスタンスに接続または割り当て失敗 |
| floatingip.port_id | Body | UUID | Floating IPが接続されたポートID |
| floatingip.id | Body | UUID | Floating IP ID |

<details><summary>例</summary>
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

### Floating IPを接続/解除する
```
PUT /v2.0/floatingips/{floatingIpId}
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| floatingIpId | URL | UUID | Floating IP ID |
| tokenId | Header | String | O | トークンID |
| floatingip | Body | Object | O | Floating IP修正リクエストオブジェクト |
| floatingip.port_id | Body | UUID | O | Floating IPを接続するポートID<br>解除するには`null`を入力 |
| floatingip.fixed_ip_address | Body | String | - | 固定IPアドレス<br>接続または解除するポートに複数のIPが割り当てられている場合、特定IPを指定するために使用 |

<details><summary>例</summary>
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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| floatingip | Body | Object | Floating IP情報オブジェクト |
| floatingip.floating_network_id | Body | UUID | Floating IPが属している外部ネットワークID |
| floatingip.router_id | Body | UUID | Floating IPが接続されたルーターID |
| floatingip.fixed_ip_address | Body | String | Floating IPが接続された固定IPアドレス |
| floatingip.floating_ip_address | Body | String | Floating IPアドレス |
| floatingip.tenant_id | Body | String | テナントID |
| floatingip.status | Body | Enum | Floating IPの状態 |
| floatingip.port_id | Body | UUID | Floating IPが接続されたポートID |
| floatingip.id | Body | UUID | Floating IP ID |

<details><summary>例</summary>
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

### Floating IPを削除する
指定したFloating IPを削除します。
```
DELETE /v2.0/floatingips/{floatingIpId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| floatingIpId | URL | UUID | O | Floating IP ID |
| tokenId | Header | String | O | トークンID |

#### レスポンス
このAPIはレスポンス本文を返しません。

---

## セキュリティグループ
### セキュリティグループリスト表示
```
GET /v2.0/security-groups
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| id | Query | UUID | - | 照会するセキュリティグループID |
| tenant_id | Query | String | - | 照会するセキュリティグループのテナントID |
| name | Query | String | - | 照会するセキュリティグループの名前 |
| sort_dir | Query | Enum | - | 照会するセキュリティグループのソート方向<br>`sort_key`で指定したフィールドを基準にソート<br>**asc**、**desc**のいずれか |
| sort_key | Query | String | - | 照会するセキュリティグループのソートキー<br>`sort_dir`で指定した方向通りにソート |
| fields | Query | String | - | 照会するセキュリティグループのフィールド名<br>例) `fields=id&fields=name` |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| security_groups | Body | Array | セキュリティグループリストオブジェクト |
| security_groups.tenant_id | Body | String | テナントID |
| security_groups.description | Body | String | セキュリティグループの説明 |
| security_groups.id | Body | UUID | セキュリティグループID |
| security_groups.security_group_rules | Body | Array | セキュリティグループルールリスト |
| security_groups.name | Body | String | セキュリティグループ名 |

<details><summary>例</summary>
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

### セキュリティグループ表示
```
GET /v2.0/security-groups/{securityGroupId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 例 | 必須 | 説明 |
|---|---|---|---|---|
| securityGroupId | Query | UUID | O | 照会するセキュリティグループID |
| tokenId | Header | String | O | トークンID |
| fields | Query | String | - | 照会するセキュリティグループのフィールド名<br>指定したフィールドのみレスポンスに返す<br>例) `fields=id&fields=name` |

#### レスポンス

| 名前 | 種類 | 例 | 説明 |
|---|---|---|---|
| security_group | Body | Object | セキュリティグループオブジェクト |
| security_group.tenant_id | Body | String | テナントID |
| security_group.description | Body | String | セキュリティグループの説明 |
| security_group.id | Body | UUID | セキュリティグループID |
| security_group.security_group_rules | Body | Array | セキュリティグループルールリスト |
| security_group.name | Body | String | セキュリティグループ名 |

<details><summary>例</summary>
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

### セキュリティグループを作成する

新しいセキュリティグループを作成します。新たに作成されたセキュリティグループは、出る方向のセキュリティグループルールを基本的に含んでいます。

```
POST /v2.0/security-groups
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| security_group | Body | Object | O | セキュリティグループ作成リクエストオブジェクト |
| description | Body | String | - | セキュリティグループの説明 |
| name | Body | String | - | セキュリティグループ名 |

<details><summary>例</summary>
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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| security_group | Body | Object | セキュリティグループオブジェクト |
| security_group.tenant_id | Body | String | テナントID |
| security_group.description | Body | String | セキュリティグループの説明 |
| security_group.id | Body | UUID | セキュリティグループID |
| security_group.security_group_rules | Body | Array | セキュリティグループルールリスト |
| security_group.name | Body | String | セキュリティグループ名 |

<details><summary>例</summary>
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

### セキュリティグループを修正する
既存セキュリティグループを修正します。
```
PUT /v2.0/security-groups/{securityGroupId}
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| securityGroupId | URL | UUID | O | セキュリティグループID |
| security_group | Body | Object | O | セキュリティグループ修正リクエストオブジェクト |
| description | Body | String | - | セキュリティグループの説明 |
| name | Body | String | - | セキュリティグループ名 |

<details><summary>例</summary>
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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| security_group | Body | Object | セキュリティグループオブジェクト |
| security_group.tenant_id | Body | String | テナントID |
| security_group.description | Body | String | セキュリティグループの説明 |
| security_group.id | Body | UUID | セキュリティグループID |
| security_group.security_group_rules | Body | Array | セキュリティグループルールリスト |
| security_group.name | Body | String | セキュリティグループ名 |

<details><summary>例</summary>
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

### セキュリティグループを削除する
指定したセキュリティグループを削除します。
```
DELETE /v2.0/security-groups/{securityGroupId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| securityGroupId | URL | UUID | O | セキュリティグループID |
| tokenId | Header | String | O | トークンID |

#### レスポンス
このAPIはレスポンス本文を返しません。

---

## セキュリティルール
### セキュリティルールリスト表示
```
GET /v2.0/security-group-rules
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| id | Query | UUID | - | 照会するセキュリティルールID |
| remote_group_id | Query | UUID | - | 照会するセキュリティルールの遠隔セキュリティグループID |
| protocol | Query | String | - | 照会するセキュリティルールのプロトコル |
| direction | Query | Enum | - | 照会するセキュリティルールが適用されるパケットの方向<br>**ingress**または**egress** |
| ethertype | Query | Enum | - | 照会するセキュリティルールのネットワークトラフィック`Ethertype`値<br>**IPv4**または**IPv6** |
| port_range_max | Query | Integer | - | 照会するセキュリティルールのポート範囲最大値 |
| port_range_min | Query | Integer | - | 照会するセキュリティルールのポート範囲最小値 |
| security_group_id | Query | UUID | - | 照会するセキュリティルールが属しているセキュリティグループID |
| tenant_id | Query | String | - | 照会するセキュリティルールのテナントID |
| remote_ip_prefix | Query | String | - | 照会するセキュリティルールの宛先IPのプリフィックス |
| description | Query | String | - | 照会するセキュリティルールの説明 |
| sort_dir | Query | Enum | - | 照会するセキュリティルールのソート方向<br>`sort_key`で指定したフィールドを基準にソート<br>**asc**、**desc**のいずれか |
| sort_key | Query | String | - | 照会するセキュリティルールのソートキー<br>`sort_dir`で指定した方向通りにソート |
| fields | Query | String | - | 照会するセキュリティルールのフィールド名<br>例) `fields=id&fields=name` |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| security_group_rules | Body | Array | セキュリティルールオブジェクトリスト |
| security_group_rules.direction | Body | Enum | セキュリティルールが適用されるパケットの方向<br>**ingress**または**egress** |
| security_group_rules.ethertype | Body | Enum | セキュリティルールのネットワークトラフィック`Ethertype`値<br>**IPv4**または**IPv6** |
| security_group_rules.protocol | Body | String | セキュリティルールのプロトコル名 |
| security_group_rules.description | Body | String | セキュリティルールの説明 |
| security_group_rules.port_range_max | Body | Integer | セキュリティルールのポート範囲最大値 |
| security_group_rules.port_range_min | Body | Integer | セキュリティルールのポート範囲最小値 |
| security_group_rules.remote_group_id | Body | UUID | セキュリティルールの遠隔セキュリティグループID |
| security_group_rules.remote_ip_prefix | Body | Enum | セキュリティルールの宛先IPのプリフィックス |
| security_group_rules.security_group_id | Body | UUID | セキュリティルールが属しているセキュリティグループID |
| security_group_rules.tenant_id | Body | String | テナントID |
| security_group_rules.id | Body | UUID | セキュリティルールID |

<details><summary>例</summary>
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

### セキュリティルール表示
```
GET /v2.0/security-group-rules/{securityGroupRuleId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| securityGroupRuleId | URL | UUID | O | セキュリティルールID |
| tokenId | Header | String | O | トークンID |
| fields | Query | String | - | 照会するセキュリティルールのフィールド名<br>例) `fields=id&fields=name` |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| security_group_rule | Body | Object | セキュリティルールオブジェクト |
| security_group_rule.direction | Body | Enum | セキュリティルールが適用されるパケットの方向<br>**ingress**または**egress** |
| security_group_rule.ethertype | Body | Enum | セキュリティルールのネットワークトラフィック`Ethertype`値<br>**IPv4**または**IPv6** |
| security_group_rule.protocol | Body | String | セキュリティルールのプロトコル名 |
| security_group_rule.description | Body | String | セキュリティルールの説明 |
| security_group_rule.port_range_max | Body | Integer | 照会するセキュリティルールのポート範囲最大値 |
| security_group_rule.port_range_min | Body | Integer | 照会するセキュリティルールのポート範囲最小値 |
| security_group_rule.remote_group_id | Body | UUID | セキュリティルールの遠隔セキュリティグループID |
| security_group_rule.remote_ip_prefix | Body | Enum | セキュリティルールの宛先IPのプリフィックス |
| security_group_rule.security_group_id | Body | UUID | セキュリティルールが属しているセキュリティグループID |
| security_group_rule.tenant_id | Body | String | テナントID |
| security_group_rule.id | Body | UUID | セキュリティルールID |

<details><summary>例</summary>
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

### セキュリティルールを作成する

新しいセキュリティグループルールを作成します。 IPv4に対するセキュリティルールのみ作成できます。

```
POST /v2.0/security-group-rules
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| security_group_rule | Body | Object | O | セキュリティルール作成リクエストオブジェクト |
| security_group_rule.remote_group_id | Body | UUID | - | セキュリティルールの遠隔セキュリティグループID |
| security_group_rule.direction | Body | Enum | O | セキュリティルールが適用されるパケットの方向<br>**ingress**、**egress** |
| security_group_rule.ethertype | Body | Enum | - | `IPv4`に指定。省略すると`IPv4`に指定 |
| security_group_rule.protocol | Body | String | - | セキュリティルールのプロトコル名。省略するとすべてのプロトコルに適用。 |
| security_group_rule.port_range_max | Body | Integer | - | セキュリティルールのポート範囲最大値 |
| security_group_rule.port_range_min | Body | Integer | - | セキュリティルールのポート範囲最小値 |
| security_group_rule.security_group_id | Body | UUID | O | セキュリティルールが属しているセキュリティグループID |
| security_group_rule.remote_ip_prefix | Body | Enum | - | セキュリティルールの宛先IPのプリフィックス |
| security_group_rule.description | Body | String | - | セキュリティルールの説明 |

<details><summary>例</summary>
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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
|---|---|---|---|
| security_group_rule | Body | Object | セキュリティルールオブジェクト |
| security_group_rule.direction | Body | Enum | セキュリティルールが適用されるパケットの方向<br>**ingress**または**egress** |
| security_group_rule.ethertype | Body | Enum | セキュリティルールのネットワークトラフィック`Ethertype`値<br>**IPv4**または**IPv6** |
| security_group_rule.protocol | Body | String | セキュリティルールのプロトコル名 |
| security_group_rule.description | Body | String | セキュリティルールの説明 |
| security_group_rule.port_range_max | Body | Integer | 照会するセキュリティルールのポート範囲最大値 |
| security_group_rule.port_range_min | Body | Integer | 照会するセキュリティルールのポート範囲最小値 |
| security_group_rule.remote_group_id | Body | UUID | セキュリティルールの遠隔セキュリティグループID |
| security_group_rule.remote_ip_prefix | Body | Enum | セキュリティルールの宛先IPのプリフィックス |
| security_group_rule.security_group_id | Body | UUID | セキュリティルールが属しているセキュリティグループID |
| security_group_rule.tenant_id | Body | String | テナントID |
| security_group_rule.id | Body | UUID | セキュリティルールID |

<details><summary>例</summary>
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

### セキュリティルールを削除する
指定したセキュリティルールを削除します。
```
DELETE /v2.0/security-group-rules/{securityGroupRuleId}
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| securityGroupRuleId | URL | UUID | O | セキュリティルールID |
| tokenId | Header | String | O | トークンID |

#### レスポンス
このAPIはレスポンス本文を返しません。
