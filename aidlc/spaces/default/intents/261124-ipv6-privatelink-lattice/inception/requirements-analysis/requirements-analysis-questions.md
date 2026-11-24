# Requirements Analysis — 質問

インテントステートメント（`ideation/intent-capture/intent-statement.md`）、実現性評価、引き継ぎ時の決定をふまえて、検証シナリオの要件を固めるための質問です。

---

## Q1. [アーキテクト] 呼び出しに使うプロトコルは何ですか？

VPC Lattice は HTTP、HTTPS、gRPC（と TLS のパススルー）を扱い、PrivateLink は TCP レベルで中継します。比較を公平にするため、同じプロトコルで比べます。

A. HTTP/1.1 の REST API
B. gRPC
C. プロトコルにはこだわらない（両方式で使える共通のもの）
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: A

## Q2. [アーキテクト] 通信の暗号化（TLS）はどうしますか？

暗号化の有無と、どこで暗号化を解くかによって、遅延の値と構成が変わります。

A. 暗号化しない（HTTP の平文。検証を単純にするため）
B. HTTPS にし、経路の途中（Lattice のサービスやロードバランサー）で暗号化を解く
C. HTTPS にし、呼び出し先の ECS サービスで暗号化を解く（途中では解かない）
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: B

## Q3. [品質] 遅延として何を測りますか？

A. リクエストを送ってから応答を受け取るまでの時間（接続済みの状態での往復時間）
B. 新しい接続を作るところから応答を受け取るまでの時間
C. A と B の両方
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: A

## Q4. [品質] 遅延の比較で、合否はどう判断しますか？

成果物の完成条件は「第三者が文書だけで検証を実施できる」ことのため、判断の基準を文書に書いておく必要があります。

A. 合否は付けず、2 方式の値（中央値、99 パーセンタイルなど）と差を記録して判断材料にする
B. 絶対値の上限を決める（例: 99 パーセンタイルで 10 ミリ秒以下。値は Other に書いてください）
C. 2 方式の差が一定以内なら「同等」とみなす（例: 中央値の差が 1 ミリ秒以内。値は Other に書いてください）
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: A

## Q5. [品質] 遅延を測るときの負荷のかけ方はどうしますか？

A. 1 件ずつ順番にリクエストを送る（負荷をかけない状態での遅延）
B. 一定数の同時リクエストをかけ続ける（負荷をかけた状態での遅延）
C. A と B の両方
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: A

## Q6. [セキュリティ] アクセス制御として、何を確かめますか？（select all that apply）

構想段階では、呼び出し元の「識別」はネットワーク上の情報に絞りました。ここでは「許可・拒否」の仕組みとして何を比べるかを決めます。

A. ネットワークのレベルでの許可・拒否（セキュリティグループなど）
B. 経路のポリシーでの許可・拒否（PrivateLink のエンドポイントポリシー、VPC Lattice の認証ポリシーなど）
C. 提供側で、接続してよい呼び出し元を承認する仕組み（PrivateLink の接続承認、Lattice のサービスネットワークへの関連付けなど）
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: A, B, C

## Q7. [セキュリティ] 提供側での呼び出し元の識別は、どこで確かめますか？

A. 呼び出し先のアプリ（ECS サービス）が受け取る情報で確かめる（送信元 IP、付加されるヘッダーなど）
B. ログ（アクセスログ、フローログなど）で確かめる
C. A と B の両方
D. Not yet defined（まだ決まっていない）
X. Other (please specify)

[Answer]: A
