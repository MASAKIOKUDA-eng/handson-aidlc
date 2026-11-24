# 使う道具の選定

入力: `inception/requirements-analysis/requirements.md`、`security-requirements.md`、`reliability-requirements.md`

## 選定 1: 遅延の測定道具 → `curl`

- **理由:**
  - Amazon Linux 2023 に最初から入っているため、IPv6 のみの環境でも追加の取得が要りません（NFR1.2）。
  - 同じコマンドに URL を複数並べると、確立済みの接続を使い回します（FR4.1）。
  - `-w '%{time_total}'` で、1 件ごとの時間を出せます。
  - 集計は `sort` と `awk` で行います。
- **ほかの候補と見送った理由:**
  - `hey`、`oha`: 集計機能は便利ですが、GitHub など IPv4 だけの取得元から入手する必要があります。
  - 自作スクリプト: 成果物にコードを含めない方針（要件分析の前提 AS1）に反します。

## 選定 2: 呼び出し先のアプリ → nginx（公式イメージを ECR に登録）

- **理由:**
  - 設定だけで、受け取った値を応答に返せます。返せる値は `$remote_addr`、`$proxy_protocol_addr`、`$proxy_protocol_tlv_aws_vpce_id`（nginx 1.23.2 以降）、リクエストヘッダーです（FR6.1）。
  - Proxy Protocol v2 を受ける待ち受け（8080）と、受けない待ち受け（8081）を、1 つのタスクで分けられます。
- **ほかの候補と見送った理由:**
  - ヘッダーをすべて返す既製のエコーサーバー: Proxy Protocol v2 の VPC エンドポイント ID を返せません。

## 選定 3: TLS の終端と名前解決

| 項目 | 内容 |
|------|------|
| PrivateLink | NLB の TLS リスナー（443）で終端し、8080（Proxy Protocol v2 あり）または 8081 に転送する |
| VPC Lattice | カスタムドメインを設定した HTTPS リスナー（443）で終端し、8081 に転送する |
| 名前 | Route 53 のプライベートホストゾーンで、`pl.<YOUR_DOMAIN>` と `lattice.<YOUR_DOMAIN>` を引けるようにする |
| 証明書 | ACM で発行する（`security-requirements.md` NFR4.3） |
