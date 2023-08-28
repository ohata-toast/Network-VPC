## Network > VPC > API v2ガイド

APIを使用するにはAPIエンドポイントとトークンなどが必要です。[API使用準備](/Compute/Compute/ko/identity-api/)を参照してAPI使用に必要な情報を準備します。

VPC APIは`network`タイプエンドポイントを利用します。正確なエンドポイントはトークン発行レスポンスの`serviceCatalog`を参照します。

| タイプ | リージョン | エンドポイント |
|---|---|---|
| network | 韓国(パンギョ)リージョン<br>韓国(ピョンチョン)リージョン<br>日本リージョン | https://kr1-api-network-infrastructure.nhncloudservice.com<br>https://kr2-api-network-infrastructure.nhncloudservice.com<br>https://jp1-api-network-infrastructure.nhncloudservice.com |

APIレスポンスにガイドに記載されていないフィールドが表示される場合があります。このようなフィールドはNHN Cloud内部用途で使用され、予告なしに変更される可能性があるため、使用しないでください。

## VPC
### VPCリストを表示
使用可能なVPCリストを返します。
```
GET /v2.0/vpcs
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。


| 名前           | 種類  | 形式   | 必須 | 説明                                                                     |
|-----------------|--------|---------|-----|---------------------------------------------------------------------------|
| tokenId         | Header | String  | O   | トークンID                                                                     |
| tenant_id       | Query  | String  | -   | 照会するネットワークが属するテナントID                                                       |
| id              | Query  | UUID    | -   | 照会するネットワークID                                                               |
| name            | Query  | String  | -   | 照会するネットワーク名                                                            |
| router:external | Query  | Boolean | -   | 照会するネットワークの外部接続の有無                                                      |
| shared          | Query  | Boolean | -   | 照会するネットワークの共有の有無                                                         |
| tenant_id       | Query  | String  | -   | 照会するネットワークが属するテナントID                                                       |
| sort_dir        | Query  | Enum    | -   | 照会するネットワークのソート方向<br>`sort_key`で指定したフィールドを基準にソート<br>**asc**、**desc**のいずれか |
| sort_key        | Query  | String  | -   | 照会するネットワークのソートキー<br>`sort_dir`で指定した方向でソート                             |


#### レスポンス
| 名前                | 種類 | 形式   | 説明              |
|----------------------|------|---------|--------------------|
| vpcs                 | Body | Array   | VPC情報オブジェクトリスト    |
| vpcs.router:external | Body | Boolean | 照会するVPCの外部接続の有無 |
| vpcs.name            | Body | String  | 照会するVPCの名前      |
| vpcs.tenant_id       | Body | UUID    | 照会するVPCが属するテナントID |
| vpcs.state           | Body | String  | 照会するVPCの状態     |
| vpcs.create_time     | Body | Date    | 照会するVPCの作成時間  |
| vpcs.cidrv4          | Body | String  | 照会するVPCのIP帯域  |
| vpcs.shared          | Body | Boolean | 照会するVPCの共有の有無   |
| vpcs.id              | Body | UUID    | 照会するVPCのID        |

<details><summary>例</summary>
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

### VPC表示
指定したVPCを照会します。
```
GET /v2.0/vpcs/{vpcId}  
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前     | 種類  | 形式  | 必須 | 説明               |
|-----------|--------|--------|-----|---------------------|
| vpcId     | URL    | String | O   | VPC ID              |
| tokenId   | Header | String | O   | トークンID               |

#### レスポンス

| 名前                                  | 種類 | 形式    | 説明                          |
|----------------------------------------|------|----------|--------------------------------|
| vpc                                    | Body | Array    | VPC情報オブジェクト                   |
| vpc.router:external                    | Body | Boolean  | 照会するVPCの外部接続の有無            |
| vpc.routingtables                      | Body | Array    | 照会するVPCのルーティングテーブル配列         |
| vpc.name                               | Body | String   | 照会するVPCの名前                  |
| vpc.subnets                            | Body | Array    | 照会するVPCのサブネットリスト             | 
| vpc.subnets.router:external            | Body | Boolean  | サブネットの外部接続の有無                |
| vpc.subnets.name                       | Body | String   | サブネットの名前                     |
| vpc.subnets.enable_dhcp                | Body | Boolean  | サブネットのDHCPが有効かどうか             |
| vpc.subnets.tenant_id                  | Body | UUID     | サブネットのテナントID                     |
| vpc.subnets.gateway                    | Body | String   | サブネットのゲートウェイIP情報            |
| vpc.subnets.routingtable               | Body | Object   | サブネットのルーティングテーブル情報             |
| vpc.subnets.routingtable.gateway_id    | Body | UUID     | ルーティングテーブルのゲートウェイID              |
| vpc.subnets.routingtable.default_table | Body | Boolean  | ルーティングテーブルのDefault Routing Tableかどうか |
| vpc.subnets.routingtable.explicit      | Body | Boolean  | ルーティングテーブルの明示的接続の有無           |
| vpc.subnets.routingtable.id            | Body | UUID     | ルーティングテーブルのID                    |
| vpc.subnets.routingtable.name          | Body | String   | ルーティングテーブルの名前                 |
| vpc.subnets.routes                     | Body | Array    | サブネットの静的パス                  |
| vpc.subnets.routes.subnet_id           | Body | UUID     | 静的パスのサブネットID                  |
| vpc.subnets.routes.tenant_id           | Body | UUID     | 静的パスのテナントID                  |
| vpc.subnets.routes.mask                | Body | Integer  | 静的パスのマスク                   |
| vpc.subnets.routes.gateway             | Body | String   | 静的パスのゲートウェイIP                |
| vpc.subnets.routes.cidr                | Body | String   | 静的パスのIP帯域                |
| vpc.subnets.routes.id                  | Body | UUID     | 静的パスのID                      |
| vpc.subnets.state                      | Body | String   | サブネットの状態                     |
| vpc.subnets.create_time                | Body | Date     | サブネットの作成時刻                  |
| vpc.subnets.available_ip_count         | Body | Interger | サブネットの割り当て可能なIP数           | 
| vpc.subnets.vpc                        | Body | Object   | サブネットのVPC情報                 |
| vpc.subnets.vpc.shared                 | Body | Boolean  | VPCの共有の有無                   |
| vpc.subnets.vpc.state                  | Body | String   | VPCの状態                     |
| vpc.subnets.vpc.id                     | Body | UUID     | VPCのID                        |
| vpc.subnets.vpc.cidrv4                 | Body | String   | 照会するVPCのIP帯域              |
| vpc.subnets.vpc.name                   | Body | String   | VPCの名前                     |
| vpc.subnets.shared                     | Body | Boolean  | サブネットの共有の有無                   |
| vpc.subnets.id                         | Body | UUID     | サブネットのID                        |
| vpc.subnets.vpc_id                     | Body | UUID     | サブネットのVPC ID                    |
| vpc.subnets.cidr                       | Body | String   | サブネットのCIDR IP                   |
| vpc.tenant_id                          | Body | UUID     | 照会するVPCが属するテナントID             |
| vpc.state                              | Body | String   | 照会するVPCの状態                 |
| vpc.create_time                        | Body | Date     | 照会するVPCの作成時間              |
| vpc.cidrv4                             | Body | String   | 照会するVPCのIPv4                  |
| vpc.shared                             | Body | Boolean  | 照会するVPCの共有の有無               |
| vpc.id                                 | Body | UUID     | 照会するVPCのID                    |

<details><summary>例</summary>
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

### VPCを作成する

```
POST /v2.0/vpcs 
X-Auth-Token: {tokenId}
```

#### リクエスト
| 名前                   | 種類  | 形式  | 必須 | 説明                                |
|-------------------------|--------|--------|-----|--------------------------------------|
| tokenId                 | Header | String | O   | トークンID                                |
| vpc                     | Body   | Object | O   | VPC作成リクエストオブジェクト                      |
| vpc.name                | Body   | String | O   | VPCの名前                            |
| vpc.cidrv4              | Body   | String | O   | VPC IP帯域                         |
| vpc.tenant_id           | Body   | UUID   |     | VPCのtenant ID                       |


<details><summary>例</summary>
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

#### レスポンス
| 名前           | 種類 | 形式   | 説明          |
|-----------------|------|---------|----------------|
| vpc             | Body | Array   | VPC情報オブジェクト   |
| vpc.name        | Body | String  | VPCの名前      |
| vpc.tenant_id   | Body | UUID    | VPCが属するテナントID |
| vpc.state       | Body | String  | VPCの状態     |
| vpc.create_time | Body | Date    | VPCの作成時間  |
| vpc.cidrv4      | Body | String  | VPCのIP帯域  |
| vpc.shared      | Body | Boolean | VPCの共有の有無   |
| vpc.id          | Body | UUID    | VPCのID        |
<details><summary>例</summary>
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

### VPCを修正する
VPCの名前とVPC IP帯域を変更します。
```
PUT /v2.0/vpcs/{vpcId}
X-Auth-Token: {tokenId}
```
#### リクエスト



| 名前      | 種類  | 形式  | 必須 | 説明        |
|------------|--------|--------|-----|--------------|
| vpcId      | URL    | String | O   | VPC ID       |
| tokenId    | Header | String | O   | トークンID        |
| vpc        | Body   | Object | O   | VPC作成リクエストオブジェクト |
| vpc.name   | Body   | String |     | VPCの名前    |
| vpc.cidrv4 | Body   | String |     | VPC IP帯域 |

<details><summary>例</summary>
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

#### レスポンス
| 名前           | 種類 | 形式   | 説明          |
|-----------------|------|---------|----------------|
| vpc             | Body | Array   | VPC情報オブジェクト   |
| vpc.name        | Body | String  | VPCの名前      |
| vpc.tenant_id   | Body | UUID    | VPCが属するテナントID |
| vpc.state       | Body | String  | VPCの状態     |
| vpc.create_time | Body | Date    | VPCの作成時間  |
| vpc.cidrv4      | Body | String  | VPCのIP帯域  |
| vpc.shared      | Body | Boolean | VPCの共有の有無   |
| vpc.id          | Body | UUID    | VPCのID        |

<details><summary>例</summary>
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


### VPCを削除する
指定したVPCを削除します。
```
DELETE /v2.0/vpcs/{vpcId}
X-Auth-Token: {tokenId}
```
#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前   | 種類  | 形式  | 必須 | 説明  |
|---------|--------|--------|-----|--------|
| vpcId   | URL    | String | O   | VPC ID |
| tokenId | Header | String | O   | トークンID  |

#### レスポンス
このAPIはレスポンス本文を返しません。

## VPCサブネット
### VPCサブネットリストの表示
使用可能なサブネットリストを返します。

```
GET /v2.0/vpcsubnets
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
|---|---|---|---|---|
| tokenId | Header | String | O | トークンID |
| id | Query | UUID | - | 照会するサブネットID |
| name | Query | String | - | 照会するサブネットの名前 |
| vpc_id | Query | UUID | - | 照会するサブネットのVPC ID |
| cidr | Query | String | - | 照会するサブネットCIDR |
| shared | Query | Boolean | - | 照会するサブネットの共有の有無 |
| sort_dir | Query | Enum | - | 照会するサブネットのソート方向<br>`sort_key`で指定したフィールドを基準にソート<br>**asc**、**desc**のいずれか |
| sort_key | Query | String | - | 照会するサブネットのソートキー<br>`sort_dir`で指定した方向でソート |


#### レスポンス

| 名前                                 | 種類 | 形式   | 説明                     |
|---------------------------------------|------|---------|---------------------------|
| vpcsubnets                            | Body | Array   | サブネット情報オブジェクト              |
| vpcsubnets.router:external            | Body | Boolean | サブネットの外部接続の有無           |
| vpcsubnets.name                       | Body | String  | サブネットの名前                |
| vpcsubnets.tenant_id                  | Body | UUID    | サブネットが属するテナントID            |
| vpcsubnets.id                         | Body | UUID    | サブネットのID                   |
| vpcsubnets.routingtable               | Body | Object  | サブネットが接続されたルーティングテーブル       |
| vpcsubnets.routingtable.gateway_id    | Body | UUID    | ルーティングテーブルのゲートウェイID         |
| vpcsubnets.routingtable.default_table | Body | Boolean | ルーティングテーブルのDefault Tableかどうか |
| vpcsubnets.routingtable.explicit      | Body | Boolean | ルーティングテーブルとの明示的接続の有無     |
| vpcsubnets.routingtable.id            | Body | UUID    | ルーティングテーブルのID               |
| vpcsubnets.routingtable.name          | Body | String  | ルーティングテーブルの名前            |
| vpcsubnets.state                      | Body | String  | サブネットの状態                |
| vpcsubnets.create_time                | Body | Date    | サブネットの作成時間             |
| vpcsubnets.available_ip_count         | Body | Integer | サブネットの使用可能なIP数      |
| vpcsubnets.vpc                        | Body | Object  | サブネットが属するVPC               |
| vpcsubnets.vpc.shared                 | Body | Boolean | VPCの共有の有無              |
| vpcsubnets.vpc.state                  | Body | String  | VPCの状態                |
| vpcsubnets.vpc.id                     | Body | UUID    | VPCのID                   |
| vpcsubnets.vpc.cidrv4                 | Body | String  | VPCのIP帯域             |
| vpcsubnets.vpc.name                   | Body | String  | VPCの名前                 |
| vpcsubnets.vpc_id                     | Body | UUID    | サブネットが属するVPCのID           |
| vpcsubnets.routes                     | Body | Array   | サブネットの静的パス             |
| vpcsubnets.routes.subnet_id           | Body | UUID    | 静的パスのサブネットID             |
| vpcsubnets.routes.tenant_id           | Body | UUID    | 静的パスのテナントID             |
| vpcsubnets.routes.mask                | Body | Integer | 静的パスのマスク              |
| vpcsubnets.routes.gateway             | Body | String  | 静的パスのゲートウェイIP           |
| vpcsubnets.routes.cidr                | Body | String  | 静的パスの対象IP帯域        |
| vpcsubnets.routes.id                  | Body | UUID    | 静的パスのID                 |
| vpcsubnets.shared                     | Body | Boolean | サブネットの共有の有無              |
| vpcsubnets.cidr                       | Body | String  | サブネットのIP帯域             |
| vpcsubnets.gateway                    | Body | String  | サブネットのゲートウェイIP             |


<details><summary>例</summary>
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

### VPCサブネットの表示
指定したサブネットを照会します。
```
GET /v2.0/vpcsubnets/{subnetId}  
X-Auth-Token: {tokenId}
```

#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前    | 種類  | 形式  | 必須 | 説明  |
|----------|--------|--------|-----|--------|
| subnetId | URL    | UUID   | O   | サブネットID |
| tokenId  | Header | String | O   | トークンID  |


#### レスポンス

| 名前                                | 種類 | 形式   | 説明                     |
|--------------------------------------|------|---------|---------------------------|
| vpcsubnet                            | Body | Array   | サブネット情報オブジェクト              |
| vpcsubnet.router:external            | Body | Boolean | サブネットの外部接続の有無           |
| vpcsubnet.name                       | Body | String  | サブネットの名前                |
| vpcsubnet.tenant_id                  | Body | UUID    | サブネットが属するテナントID            |
| vpcsubnet.id                         | Body | UUID    | サブネットのID                   |
| vpcsubnet.routingtable               | Body | Object  | サブネットが接続されたルーティングテーブル       |
| vpcsubnet.routingtable.gateway_id    | Body | UUID    | ルーティングテーブルのゲートウェイID         |
| vpcsubnet.routingtable.default_table | Body | Boolean | ルーティングテーブルのDefault Tableかどうか |
| vpcsubnet.routingtable.explicit      | Body | Boolean | ルーティングテーブルとの明示的接続の有無     |
| vpcsubnet.routingtable.id            | Body | UUID    | ルーティングテーブルのID               |
| vpcsubnet.routingtable.name          | Body | String  | ルーティングテーブルの名前            |
| vpcsubnet.state                      | Body | String  | サブネットの状態                |
| vpcsubnet.create_time                | Body | Date    | サブネットの作成時間             |
| vpcsubnet.available_ip_count         | Body | Integer | サブネットの使用可能なIP数      |
| vpcsubnet.vpc                        | Body | Object  | サブネットが属するVPC               |
| vpcsubnet.vpc.shared                 | Body | Boolean | VPCの共有の有無              |
| vpcsubnet.vpc.state                  | Body | String  | VPCの状態                |
| vpcsubnet.vpc.id                     | Body | UUID    | VPCのID                   |
| vpcsubnet.vpc.cidrv4                 | Body | String  | VPCのIP帯域             |
| vpcsubnet.vpc.name                   | Body | String  | VPCの名前                 |
| vpcsubnet.vpc_id                     | Body | UUID    | サブネットが属するVPCのID           |
| vpcsubnet.routes                     | Body | Array   | サブネットの静的パス             |
| vpcsubnet.routes.subnet_id           | Body | UUID    | 静的パスのサブネットID             |
| vpcsubnet.routes.tenant_id           | Body | UUID    | 静的パスのテナントID             |
| vpcsubnet.routes.mask                | Body | Integer | 静的パスのマスク              |
| vpcsubnet.routes.gateway             | Body | String  | 静的パスのゲートウェイIP           |
| vpcsubnet.routes.cidr                | Body | String  | 静的パスの対象IP帯域        |
| vpcsubnet.routes.id                  | Body | UUID    | 静的パスのID                 |
| vpcsubnet.shared                     | Body | Boolean | サブネットの共有の有無              |
| vpcsubnet.cidr                       | Body | String  | サブネットのIP帯域             |
| vpcsubnet.gateway                    | Body | String  | サブネットのゲートウェイIP             |


<details><summary>例</summary>
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

### VPCサブネットを作成する
新しいサブネットを作成します。
```
POST /v2.0/vpcsubnets  
X-Auth-Token: {tokenId}
```
#### リクエスト

| 名前               | 種類  | 形式  | 必須 | 説明           |
|---------------------|--------|--------|-----|-----------------|
| tokenId             | Header | String | O   | トークンID           |
| vpcsubnet           | Body   | Object | O   | サブネット情報オブジェクト    |
| vpcsubnet.vpc_id    | Body   | UUID   | O   | サブネットが割り当てられるVPC ID |
| vpcsubnet.cidr      | Body   | String | O   | サブネットのIP帯域   |
| vpcsubnet.name      | Body   | String | O   | サブネットの名前      |
| vpcsubnet.tenant_id | Body   | UUID   |     | サブネットが割り当てられるテナントID |

<details><summary>例</summary>
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

#### レスポンス

| 名前                 | 種類 | 形式   | 説明             |
|-----------------------|------|---------|-------------------|
| vpcsubnet             | Body | Array   | サブネット情報オブジェクト      |
| vpcsubnet.name        | Body | String  | サブネットの名前        |
| vpcsubnet.tenant_id   | Body | String  | サブネットが属するテナントID    |
| vpcsubnet.id          | Body | UUID    | サブネットのID           |
| vpcsubnet.state       | Body | String  | サブネットの状態        |
| vpcsubnet.create_time | Body | Date    | サブネットの作成時間     |
| vpcsubnet.vpc_id      | Body | String  | サブネットが属するVPCのID   |
| vpcsubnet.shared      | Body | Boolean | サブネットの共有の有無      |
| vpcsubnet.cidr        | Body | String  | サブネットのIP帯域     |
| vpcsubnet.gateway     | Body | String  | サブネットのゲートウェイIP |

<details><summary>例</summary>
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

### VPCサブネットを修正する
サブネットの名前を変更します。
```
PUT /v2.0/vpcsubnets/{subnetId}  
X-Auth-Token: {tokenId}
```
#### リクエスト
| 名前          | 種類  | 形式  | 必須 | 説明     |
|----------------|--------|--------|-----|-----------|
| subnetId       | URL    | UUID   | O   | サブネットID    |
| tokenId        | Header | String | O   | トークンID     |
| vpcsubnet      | Body   | Object | O   | サブネット情報オブジェクト |
| vpcsubnet.name | Body   | String | O   | サブネットの名前 |

<details><summary>例</summary>
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

#### レスポンス

| 名前                 | 種類 | 形式   | 説明             |
|-----------------------|------|---------|-------------------|
| vpcsubnet             | Body | Array   | サブネット情報オブジェクト      |
| vpcsubnet.name        | Body | String  | サブネットの名前        |
| vpcsubnet.tenant_id   | Body | String  | サブネットが属するテナントID    |
| vpcsubnet.id          | Body | UUID    | サブネットのID           |
| vpcsubnet.state       | Body | String  | サブネットの状態        |
| vpcsubnet.create_time | Body | Date    | サブネットの作成時間     |
| vpcsubnet.vpc_id      | Body | String  | サブネットが属するVPCのID   |
| vpcsubnet.shared      | Body | Boolean | サブネットの共有の有無      |
| vpcsubnet.cidr        | Body | String  | サブネットのIP帯域     |
| vpcsubnet.gateway     | Body | String  | サブネットのゲートウェイIP |

<details><summary>例</summary>
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

### VPCサブネットをルーティングテーブルに接続する
サブネットを特定ルーティングテーブルに明示的に接続します。
```
PUT /v2.0/vpcsubnets/{subnetId}/attach_routingtable
X-Auth-Token: {tokenId}
```
#### リクエスト
| 名前    | 種類  | 形式  | 必須 | 説明  |
|----------|--------|--------|-----|--------|
| subnetId | URL    | UUID   | O   | サブネットID |
| tokenId  | Header | String | O   | トークンID  |
| routingtable_id | Body | UUID | O   | 接続するルーティングテーブルID |

<details><summary>例</summary>
<p>

```json
{
   "routingtable_id": "c577a618-1acb-444d-a82e-aeb4c6fce01e"
}
```

</p>
</details>

#### レスポンス

| 名前                                | 種類 | 形式   | 説明                     |
|--------------------------------------|------|---------|---------------------------|
| vpcsubnet                            | Body | Array   | サブネット情報オブジェクト              |
| vpcsubnet.router:external            | Body | Boolean | サブネットの外部接続の有無           |
| vpcsubnet.name                       | Body | String  | サブネットの名前                |
| vpcsubnet.tenant_id                  | Body | UUID    | サブネットが属するテナントID            |
| vpcsubnet.id                         | Body | UUID    | サブネットのID                   |
| vpcsubnet.routingtable               | Body | Object  | サブネットが接続されたルーティングテーブル       |
| vpcsubnet.routingtable.gateway_id    | Body | UUID    | ルーティングテーブルのゲートウェイID         |
| vpcsubnet.routingtable.default_table | Body | Boolean | ルーティングテーブルのDefault Tableかどうか |
| vpcsubnet.routingtable.explicit      | Body | Boolean | ルーティングテーブルとの明示的接続の有無     |
| vpcsubnet.routingtable.id            | Body | UUID    | ルーティングテーブルのID               |
| vpcsubnet.routingtable.name          | Body | String  | ルーティングテーブルの名前            |
| vpcsubnet.state                      | Body | String  | サブネットの状態                |
| vpcsubnet.create_time                | Body | Date    | サブネットの作成時間             |
| vpcsubnet.available_ip_count         | Body | Integer | サブネットの使用可能なIP数      |
| vpcsubnet.vpc                        | Body | Object  | サブネットが属するVPC               |
| vpcsubnet.vpc.shared                 | Body | Boolean | VPCの共有の有無              |
| vpcsubnet.vpc.state                  | Body | String  | VPCの状態                |
| vpcsubnet.vpc.id                     | Body | UUID    | VPCのID                   |
| vpcsubnet.vpc.cidrv4                 | Body | String  | VPCのIP帯域             |
| vpcsubnet.vpc.name                   | Body | String  | VPCの名前                 |
| vpcsubnet.vpc_id                     | Body | UUID    | サブネットが属するVPCのID           |
| vpcsubnet.routes                     | Body | Array   | サブネットの静的パス             |
| vpcsubnet.routes.subnet_id           | Body | UUID    | 静的パスのサブネットID             |
| vpcsubnet.routes.tenant_id           | Body | UUID    | 静的パスのテナントID             |
| vpcsubnet.routes.mask                | Body | Integer | 静的パスのマスク              |
| vpcsubnet.routes.gateway             | Body | String  | 静的パスのゲートウェイIP           |
| vpcsubnet.routes.cidr                | Body | String  | 静的パスの対象IP帯域        |
| vpcsubnet.routes.id                  | Body | UUID    | 静的パスのID                 |
| vpcsubnet.shared                     | Body | Boolean | サブネットの共有の有無              |
| vpcsubnet.cidr                       | Body | String  | サブネットのIP帯域             |
| vpcsubnet.gateway                    | Body | String  | サブネットのゲートウェイIP             |



<details><summary>例</summary>
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

### VPCサブネットとルーティングテーブルの接続を解除する
サブネットとルーティングテーブルの明示的接続を解除します。

```
PUT /v2.0/vpcsubnets/{subnetId}/detach_routingtable
X-Auth-Token: {tokenId}
```
#### リクエスト
このAPIはリクエスト本文を要求しません。

| 名前    | 種類  | 形式  | 必須 | 説明  |
|----------|--------|--------|-----|--------|
| subnetId | URL    | UUID   | O   | サブネットID |
| tokenId  | Header | String | O   | トークンID  |

#### レスポンス

| 名前                                | 種類 | 形式   | 説明                    |
|--------------------------------------|------|---------|--------------------------|
| vpcsubnet                            | Body | Array   | サブネット情報オブジェクト             |
| vpcsubnet.router:external            | Body | Boolean | サブネットの外部接続の有無          |
| vpcsubnet.name                       | Body | String  | サブネットの名前               |
| vpcsubnet.enable_dhcp                | Body | Boolean | サブネットのDHCPが有効かどうか       |
| vpcsubnet.tenant_id                  | Body | UUID    | サブネットが属するテナントID           |
| vpcsubnet.routingtable               | Body | Object  | サブネットのルーティングテーブル          |
| vpcsubnet.routingtable.gateway_id    | Body | UUID    | ルーティングテーブルのゲートウェイID        |
| vpcsubnet.routingtable.default_table | Body | Boolean | ルーティングテーブルのDefault Tableかどうか |
| vpcsubnet.routingtable.explicit      | Body | Boolean | ルーティングテーブルの明示的接続の有無     |
| vpcsubnet.routingtable.id            | Body | UUID    | ルーティングテーブルのID              |
| vpcsubnet.routingtable.name          | Body | String  | ルーティングテーブルの名前           |
| vpcsubnet.gateway                    | Body | String  | サブネットに接続されたゲートウェイIP        |
| vpcsubnet.routes                     | Body | Array   | サブネットの静的パス            |
| vpcsubnet.routes.subnet_id           | Body | UUID    | 静的パスのサブネットID            |
| vpcsubnet.routes.tenant_id           | Body | UUID    | 静的パスのテナントID            |
| vpcsubnet.routes.mask                | Body | Integer | 静的パスのマスク             |
| vpcsubnet.routes.gateway             | Body | String  | 静的パスのゲートウェイIP          |
| vpcsubnet.routes.cidr                | Body | String  | 静的パスのIP帯域          |
| vpcsubnet.routes.id                  | Body | UUID    | 静的パスのID                |
| vpcsubnet.state                      | Body | String  | サブネットの状態               |
| vpcsubnet.create_time                | Body | Date    | サブネットの作成時間            |
| vpcsubnet.available_ip_count         | Body | Integer | サブネットが使用可能なIP数     |
| vpcsubnet.vpc                        | Body | Object  | サブネットが属するVPC              |
| vpcsubnet.vpc.shared                 | Body | Boolean | VPCの共有の有無             |
| vpcsubnet.vpc.state                  | Body | String  | VPCの状態               |
| vpcsubnet.vpc.id                     | Body | UUID    | VPCのID                  |
| vpcsubnet.vpc.cidrv4                 | Body | String  | VPCのIP帯域            |
| vpcsubnet.vpc.name                   | Body | String  | VPCの名前                |
| vpcsubnet.shared                     | Body | Boolean | サブネットの共有の有無             |
| vpcsubnet.id                         | Body | UUID    | サブネットのID                  |
| vpcsubnet.vpc_id                     | Body | UUID    | サブネットが属するVPCのID          |
| vpcsubnet.cidr                       | Body | String  | サブネットのIP帯域            |


<details><summary>例</summary>
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

### VPCサブネットを削除する

指定したサブネットを削除します。

```
DELETE /v2.0/vpcsubnets/{subnetId}  
X-Auth-Token: {tokenId}
```

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前    | 種類  | 形式  | 必須 | 説明  |
|----------|--------|--------|-----|--------|
| subnetId | URL    | UUID   | O   | サブネットID |
| tokenId  | Header | String | O   | トークンID  |

#### レスポンス
このAPIはレスポンス本文を返しません。

## ルーティングテーブル

### ルーティングテーブルリスト表示

使用可能なルーティングテーブルのリストを返します。

```
GET /v2.0/routingtables
X-Auth-Token: {tokenId}
```

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前     | 種類   | 形式   | 必須 | 説明   |
|----------|--------|--------|-----|--------|
| tokenId | Header | String | O | トークンID |
| tenant_id | Query | String | - | 照会するルーティングテーブルが属するテナントID |
| id | Query | UUID | - | 照会するルーティングテーブルID |
| name | Query | String | - | 照会するルーティングテーブル名 |
| default_table | Query | Boolean | - | 照会するルーティングテーブルの基本ルーティングテーブルかどうか |
| gateway_id | Query | UUID | - | 照会するルーティングテーブルに接続されたインターネットゲートウェイID |
| distributed | Query | Boolean | - | 照会するルーティングテーブルのルーティング方式<br>`true`:分散型、`false`:中央集中型 | 
| detail | Query | Boolean | - | 照会するルーティングテーブルの詳細情報を含むかどうか |
| sort_dir | Query  | Enum    | -   | 照会するネットワークのソート方向<br>`sort_key`で指定したフィールドを基準にソート<br>**asc**、**desc**のいずれか |
| sort_key | Query  | String  | -   | 照会するネットワークのソートキー<br>`sort_dir`で指定した方向でソート |

#### レスポンス

##### **detail**クエリパラメータがないか値が`false`の場合

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| routingtables | Body | Array | ルーティングテーブル情報オブジェクトリスト |
| routingtables.id | Body | UUID | ルーティングテーブルID |
| routingtables.name | Body | String | ルーティングテーブル名 |
| routingtables.default_table | Body | Boolean | 基本ルーティングテーブルかどうか |
| routingtables.distributed | Body | Boolean | 照会するルーティングテーブルのルーティング方式<br>`true`:分散型、`false`:中央集中型 |
| routingtables.gateway_id | Body | UUID | ルーティングテーブルにインターネットゲートウェイが接続されている場合、そのインターネットゲートウェイのID |
| routingtables.tenant_id | Body | String | ルーティングテーブルが属するテナントID |
| routingtables.state | Body | String | ルーティングテーブルの状態。現在は`available`状態のみ存在 |
| routingtable.create_time | Date | ルーティングテーブルの作成時間 |

<details><summary>例</summary>
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

##### **detail**クエリパラメータ値が`true`の場合

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| routingtables | Body | Array | ルーティングテーブル情報オブジェクトリスト |
| routingtables.id | Body | UUID | ルーティングテーブルID |
| routingtables.name | Body | String | ルーティングテーブル名 |
| routingtables.default_table | Body | Boolean | 基本ルーティングテーブルかどうか |
| routingtables.distributed | Body | Boolean | 照会するルーティングテーブルのルーティング方式<br>`true`:分散型、`false`:中央集中型 |
| routingtables.gateway_id | Body | UUID | ルーティングテーブルにインターネットゲートウェイが接続されている場合、そのインターネットゲートウェイのID |
| routingtables.tenant_id | Body | String | ルーティングテーブルが属するテナントID |
| routingtables.state | Body | String | ルーティングテーブルの状態。現在は`available`状態のみ存在 |
| routingtables.vpcs | Body | Array | ルーティングテーブルが属するVPC情報オブジェクトリスト |
| routingtables.vpcs.id | Body | UUID | ルーティングテーブルが属するVPC ID |
| routingtables.vpcs.name | Body | String | ルーティングテーブルが属するVPC名 |
| routingtables.subnets | Body | Array | ルーティングテーブルに接続されたサブネット情報オブジェクトリスト | 
| routingtables.subnets.id | Body | UUID | ルーティングテーブルに接続されたサブネットID | 
| routingtables.subnet.name | Body | UUID | ルーティングテーブルに接続されたサブネット名 |
| routingtables.create_time | Date | ルーティングテーブルの作成時間 |

<details><summary>例</summary>
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

### ルーティングテーブル表示

指定したルーティングテーブルを照会します。

```
GET /v2.0/routingtables/{routingtableId}
X-Auth-Token: {tokenId}
```

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| routingtableId | URL | UUID | O | 照会するルーティングテーブルID |
| tokenId | Header | String | O | トークンID |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| routingtable | Body | Object | ポート情報オブジェクト |
| routingtable.id | Body | UUID | ルーティングテーブルID |
| routingtable.name | Body | String | ルーティングテーブル名 |
| routingtable.default_table | Body | Boolean | 基本ルーティングテーブルかどうか |
| routingtable.distributed | Body | Boolean | 照会するルーティングテーブルのルーティング方式<br>`true`:分散型、 `false`:中央集中型 |
| routingtable.gateway_id | Body | UUID | ルーティングテーブルにインターネットゲートウェイが接続されている場合、該当インターネットゲートウェイのID |
| routingtable.tenant_id | Body | String | ルーティングテーブルが属するテナントID |
| routingtable.state | Body | String | ルーティングテーブルの状態。現在は`available`状態のみ存在 |
| routingtable.vpcs | Body | Array | ルーティングテーブルが属するVPC情報オブジェクトリスト |
| routingtable.vpcs.id | Body | UUID | ルーティングテーブルが属するVPC ID |
| routingtable.subnets | Body | Array | ルーティングテーブルに接続されているサブネット情報オブジェクトリスト | 
| routingtable.subnets.id | Body | UUID | ルーティングテーブルに接続されているサブネットID | 
| routingtable.routes | Body | Array | ルーティングテーブルに設定されたルート情報オブジェクトリスト |
| routingtable.routes.id | Body | UUID | ルートID |
| routingtable.routes.cidr | Body | String | ルート目的地CIDR |
| routingtable.routes.mask | Body | String | ルート目的地CIDRのネットマスク |
| routingtable.routes.gateway | Body | String | ルートゲートウェイIP |
| routingtable.routes.gateway_id | Body | String | インターネットゲートウェイに向かうルートの場合、インターネットゲートウェイID |
| routingtable.routes.routingtable_id | Body | String | ルートが属するルーティングテーブルID |
| routingtable.routes.tenant_id | Body | String | ルートが属するテナントID |
| routingtable.create_time | Date | ルーティングテーブルの作成時間 |

<details><summary>例</summary>
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

### ルーティングテーブルを作成する

新しいルーティングテーブルを作成します。

```
POST /v2.0/routingtables
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| routingtable | Body | Object | O | ルーティングテーブル作成リクエストオブジェクト |
| routingtable.name | Body | String | O | ルーティングテーブル名 |
| routingtable.vpc_id | Body | UUID | O | ルーティングテーブルが属するVPC ID |
| routingtable.distributed | Body | Boolean | - | ルーティングテーブルのルーティング方式<br>`true`:分散型、 `false`:中央集中型(デフォルト値: `true`) |

<details><summary>例</summary>
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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| routingtable | Body | Object | 作成されたルーティングテーブル情報オブジェクト |
| id | Body | UUID | ルーティングテーブルID |
| name | Body | String | ルーティングテーブル名 |
| default_table | Body | Boolean | 基本ルーティングテーブルかどうか |
| distributed | Body | Boolean | ルーティングテーブルのルーティング方式<br>`true`:分散型、 `false`:中央集中型 |
| state | Body | String | ルーティングテーブルの状態。現在は`"available"`値のみ使用 |
| tenant_id | Body | String | ルーティングテーブルが属するテナントID |
| create_time | Body | Date | ルーティングテーブル作成時間 |

<details><summary>例</summary>
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

### ルーティングテーブルを修正する

ルーティングテーブルの情報を修正します。ルーティングテーブルの名前とルーティング方式(分散型/中央集中型)を変更できます。

```
PUT /v2.0/routingtables/{routingtableId}
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| routingtableId | URL | UUID | O | 修正するルーティングテーブルID |
| tokenId | Header | String | O | トークンID |
| routingtable | Body | Object | O | ルーティングテーブル情報オブジェクト |
| routingtable.name | Body | String | - | 変更するルーティングテーブル名 |
| routingtable.distributed | Body | Boolean | - | 変更するルーティングテーブルのルーティング方式<br>`true`:分散型、 `false`:中央集中型 | 


<details><summary>例</summary>
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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| routingtable | Body | Object | 作成されたルーティングテーブル情報オブジェクト |
| id | Body | UUID | ルーティングテーブルID |
| name | Body | String | ルーティングテーブル名 |
| default_table | Body | Boolean | 基本ルーティングテーブルかどうか |
| distributed | Body | Boolean | ルーティングテーブルのルーティング方式<br>`true`:分散型、 `false`:中央集中型 |
| state | Body | String | ルーティングテーブルの状態。現在は`"available"`値のみ使用 |
| tenant_id | Body | String | ルーティングテーブルが属するテナントID |
| create_time | Body | Date | ルーティングテーブル作成時間 |

<details><summary>例</summary>
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


### ルーティングテーブルにインターネットゲートウェイを接続する

ルーティングテーブルにインターネットゲートウェイを接続します。

```
PUT /v2.0/routingtables/{routingtableId}/attach_gateway
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| routingtableId | URL | UUID | O | 修正するルーティングテーブルID |
| tokenId | Header | String | O | トークンID |
| gateway_id | Body | UUID | O | ルーティングテーブルに接続するインターネットゲートウェイのID |

<details><summary>例</summary>
<p>

```json
{
  "gateway_id": "615d1cb1-fe54-4505-8a39-35faa6c868cd"
}
```

</p>
</details>

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| routingtable | Body | Object | ポート情報オブジェクト |
| routingtable.id | Body | UUID | ルーティングテーブルID |
| routingtable.name | Body | String | ルーティングテーブル名 |
| routingtable.default_table | Body | Boolean | 基本ルーティングテーブルかどうか |
| routingtable.distributed | Body | Boolean | 照会するルーティングテーブルのルーティング方式<br>`true`:分散型、 `false`:中央集中型 |
| routingtable.gateway_id | Body | UUID | ルーティングテーブルにインターネットゲートウェイが接続されている場合、該当インターネットゲートウェイのID |
| routingtable.tenant_id | Body | String | ルーティングテーブルが属するテナントID |
| routingtable.state | Body | String | ルーティングテーブルの状態。現在は`available`状態のみ存在 |
| routingtable.vpcs | Body | Array | ルーティングテーブルが属するVPC情報オブジェクトリスト |
| routingtable.vpcs.id | Body | UUID | ルーティングテーブルが属するVPC ID |
| routingtable.subnets | Body | Array | ルーティングテーブルに接続されているサブネット情報オブジェクトリスト | 
| routingtable.subnets.id | Body | UUID | ルーティングテーブルに接続されているサブネットID | 
| routingtable.routes | Body | Array | ルーティングテーブルに設定されたルート情報オブジェクトリスト |
| routingtable.routes.id | Body | UUID | ルートID |
| routingtable.routes.cidr | Body | String | ルート目的地CIDR |
| routingtable.routes.mask | Body | String | ルート目的地CIDRのネットマスク |
| routingtable.routes.gateway | Body | String | ルートゲートウェイIP |
| routingtable.routes.gateway_id | Body | String | インターネットゲートウェイに向かうルートの場合、インターネットゲートウェイID |
| routingtable.routes.routingtable_id | Body | String | ルートが属するルーティングテーブルID |
| routingtable.routes.tenant_id | Body | String | ルートが属するテナントID |
| routingtable.create_time | Date | ルーティングテーブルの作成時間 |

<details><summary>例</summary>
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

### ルーティングテーブルのインターネットゲートウェイを接続解除する

ルーティングテーブルと接続されたインターネットゲートウェイとの接続を解除します。

```
PUT /v2.0/routingtables/{routingtableId}/detach_gateway
X-Auth-Token: {tokenId}
```

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| routingtableId | URL | UUID | O | 修正するルーティングテーブルID |
| tokenId | Header | String | O | トークンID |


#### レスポンス
| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| routingtable | Body | Object | ポート情報オブジェクト |
| routingtable.id | Body | UUID | ルーティングテーブルID |
| routingtable.name | Body | String | ルーティングテーブル名 |
| routingtable.default_table | Body | Boolean | 基本ルーティングテーブルかどうか |
| routingtable.distributed | Body | Boolean | 照会するルーティングテーブルのルーティング方式<br>`true`:分散型、 `false`:中央集中型 |
| routingtable.tenant_id | Body | String | ルーティングテーブルが属するテナントID |
| routingtable.state | Body | String | ルーティングテーブルの状態。現在は`available`状態のみ存在 |
| routingtable.vpcs | Body | Array | ルーティングテーブルが属するVPC情報オブジェクトリスト |
| routingtable.vpcs.id | Body | UUID | ルーティングテーブルが属するVPC ID |
| routingtable.subnets | Body | Array | ルーティングテーブルに接続されているサブネット情報オブジェクトリスト | 
| routingtable.subnets.id | Body | UUID | ルーティングテーブルに接続されているサブネットID | 
| routingtable.routes | Body | Array | ルーティングテーブルに設定されたルート情報オブジェクトリスト |
| routingtable.routes.id | Body | UUID | ルートID |
| routingtable.routes.cidr | Body | String | ルート目的地CIDR |
| routingtable.routes.mask | Body | String | ルート目的地CIDRのネットマスク |
| routingtable.routes.gateway | Body | String | ルートゲートウェイIP |
| routingtable.routes.gateway_id | Body | String | インターネットゲートウェイに向かうルートの場合、インターネットゲートウェイID |
| routingtable.routes.routingtable_id | Body | String | ルートが属するルーティングテーブルID |
| routingtable.routes.tenant_id | Body | String | ルートが属するテナントID |
| routingtable.create_time | Date | ルーティングテーブルの作成時間 |

<details><summary>例</summary>
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

### ルーティングテーブルを基本ルーティングテーブルに指定する

ルーティングテーブルを基本ルーティングテーブルに指定します。

```
PUT /v2.0/routingtables/{routingtableId}/set_as_default
X-Auth-Token: {tokenId}
```

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| routingtableId | URL | UUID | O | 修正するルーティングテーブルID |
| tokenId | Header | String | O | トークンID |


#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| routingtable | Body | Object | ポート情報オブジェクト |
| routingtable.id | Body | UUID | ルーティングテーブルID |
| routingtable.name | Body | String | ルーティングテーブル名 |
| routingtable.default_table | Body | Boolean | 基本ルーティングテーブルかどうか |
| routingtable.distributed | Body | Boolean | 照会するルーティングテーブルのルーティング方式<br>`true`:分散型、 `false`:中央集中型 |
| routingtable.gateway_id | Body | UUID | ルーティングテーブルにインターネットゲートウェイが接続されている場合、該当インターネットゲートウェイのID |
| routingtable.tenant_id | Body | String | ルーティングテーブルが属するテナントID |
| routingtable.state | Body | String | ルーティングテーブルの状態。現在は`available`状態のみ存在 |
| routingtable.vpcs | Body | Array | ルーティングテーブルが属するVPC情報オブジェクトリスト |
| routingtable.vpcs.id | Body | UUID | ルーティングテーブルが属するVPC ID |
| routingtable.subnets | Body | Array | ルーティングテーブルに接続されているサブネット情報オブジェクトリスト | 
| routingtable.subnets.id | Body | UUID | ルーティングテーブルに接続されているサブネットID | 
| routingtable.routes | Body | Array | ルーティングテーブルに設定されたルート情報オブジェクトリスト |
| routingtable.routes.id | Body | UUID | ルートID |
| routingtable.routes.cidr | Body | String | ルート目的地CIDR |
| routingtable.routes.mask | Body | String | ルート目的地CIDRのネットマスク |
| routingtable.routes.gateway | Body | String | ルートゲートウェイIP |
| routingtable.routes.gateway_id | Body | String | インターネットゲートウェイに向かうルートの場合、インターネットゲートウェイID |
| routingtable.routes.routingtable_id | Body | String | ルートが属するルーティングテーブルID |
| routingtable.routes.tenant_id | Body | String | ルートが属するテナントID |
| routingtable.create_time | Date | ルーティングテーブルの作成時間 |

<details><summary>例</summary>
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


### ルーティングテーブルを削除する

ルーティングテーブルを削除します。基本ルーティングテーブルに指定されたルーティングテーブルは削除できず、これはVPC削除時に一緒に削除されます。

```
DELETE /v2.0/routingtables/{routingtableId}
X-Auth-Token: {tokenId}
```

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| routingtableId | URL | UUID | O | 削除するルーティングテーブルID |
| tokenId | Header | String | O | トークンID |


#### レスポンス

このAPIはレスポンス本文を返しません。

## ルート
### ルートリスト表示

ルーティングテーブルに設定されたルートリストを返します。

```
GET /v2.0/routes
X-Auth-Token: {tokenId}
```

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| id | Query | UUID | - | ルートID |
| cidr | Query | String | - | ルート目的地CIDR |
| mask | Query | Integer | -  | ルート目的地CIDRのネットマスク(0～32) |
| gateway | Query | String | - | ルートゲートウェイIP |
| routingtable_id |  Query | String | - | ルートが設定されたルーティングテーブルのID|
| gateway_id |  Query | String | - | インターネットゲートウェイID |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| routes | Body | Array | ルート情報オブジェクトリスト |
| routes.id | Body | UUID | ルートID |
| routes.cidr | Body | String | ルート目的地CIDR | 
| routes.mask | Body | Integer | ルート目的地CIDRのネットマスク |
| routes.gateway | Body | String | ルートゲートウェイIP。ローカルルートの場合"local" |
| routes.gateway_id | Body | UUID | ルーティングテーブルにインターネットゲートウェイを接続して自動的に作成されたルートの場合、インターネットゲートウェイのID |
| routes.tenant_id | Body | String | ルートが属するテナントID |

<details><summary>例</summary>
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

### ルート表示

指定したルートを照会します。

```
GET /v2.0/routes/{routeId}
X-Auth-Token: {tokenId}
```

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| routeId | URL | UUID | O | 照会するルートID |
| tokenId | Header | String | O | トークンID |

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| route | Body | Array | ルート情報オブジェクト |
| route.id | Body | UUID | ルートID |
| route.cidr | Body | String | ルート目的地CIDR | 
| route.mask | Body | Integer | ルート目的地CIDRのネットマスク |
| route.gateway | Body | String | ルートゲートウェイIP。ローカルルートの場合は"local" |
| route.gateway_id | Body | UUID | ルーティングテーブルにインターネットゲートウェイを接続して自動的に作成されたルートの場合、インターネットゲートウェイのID |
| route.tenant_id | Body | String | ルートが属するテナントID |

<details><summary>例</summary>
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


### ルートを作成する

ルーティングテーブルに新規ルートを追加します。

```
POST /v2.0/routes
X-Auth-Token: {tokenId}
```

#### リクエスト

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| tokenId | Header | String | O | トークンID |
| routingtable_id | Body | UUID | O | ルートを追加するルーティングテーブルID |
| cidr | Body | String | O | ルート目的地CIDR |
| gateway| Body | String | O | ルートゲートウェイIP |

<details><summary>例</summary>
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

#### レスポンス

| 名前 | 種類 | 形式 | 説明 |
| --- | --- | --- | --- |
| route | Body | Array | ルート情報オブジェクト |
| route.id | Body | UUID | ルートID |
| route.cidr | Body | String | ルート目的地CIDR | 
| route.mask | Body | Integer | ルート目的地CIDRのネットマスク |
| route.gateway | Body | String | ルートゲートウェイIP |
| route.tenant_id | Body | String | ルートが属するテナントID |

<details><summary>例</summary>
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

### ルートを削除する

指定したルートを削除します。 `gateway`項目が"local"である場合や、インターネットゲートウェイ接続により自動的に追加されたルート(`gateway_id`値存在)は削除できません。

```
DELETE /v2.0/routes/{routeId}
X-Auth-Token: {tokenId}
```

#### リクエスト

このAPIはリクエスト本文を要求しません。

| 名前 | 種類 | 形式 | 必須 | 説明 |
| --- | --- | --- | --- | --- |
| routeId | URL | UUID | O | 削除するルートID |
| tokenId | Header | String | O | トークンID |


#### レスポンス

このAPIはレスポンス本文を返しません。


---
