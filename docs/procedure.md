# 検証 B の手順（AI-DLC あり）

aidlc-workflows のコマンドは、公式ガイド [Install and Lifecycle](https://awslabs.github.io/aidlc-workflows/guide/18-install-and-lifecycle/) の記載に従っています（2026-11-24 時点で確認）。

## 0. 事前準備

### 0.1 aidlc-workflows 2.11.3 を入れる（macOS / Linux / WSL）

インストーラーは、いったんファイルに保存し、**中身を確認してから** 実行します（`curl | sh` で直接実行しない）。

```bash
tmp="$(mktemp -d)"
curl -fsSL \
  https://github.com/awslabs/aidlc-workflows/releases/latest/download/install.sh \
  -o "$tmp/install.sh"
less "$tmp/install.sh"                 # 中身を確認する
sh "$tmp/install.sh" --version 2.11.3  # バージョンを固定してインストール
rm -rf "$tmp"
export PATH="$HOME/.local/bin:$PATH"
```

- `--version` には `v` を付けずに `2.11.3` と書く
- GitHub CLI（`gh`）が入っていれば、インストーラーがリリースの署名を検証する（`gh` がなくてもインストールは止まらない）
- インストール時の出力に表示されるバージョンを、[evaluation-sheet.md](evaluation-sheet.md) に書く

### 0.2 このリポジトリに設定する

```bash
git clone https://github.com/MASAKIOKUDA-eng/handson-aidlc.git
cd handson-aidlc
git switch -c run/b-YYYYMMDD

aidlc config --harness claude --dry-run   # 何が作られるかを先に確認する
aidlc config --harness claude
aidlc config --pin 2.11.3 --yes           # プロジェクトのバージョンを固定する
aidlc doctor                              # 問題がないことを確認する
```

設定したファイルは、検証の前に 1 回コミットしておきます（検証で増えたものと区別するため）。

```bash
git add -A
git commit -m "chore: aidlc-workflows 2.11.3 を Claude Code 用に設定"
```

### 0.3 記録の準備

1. Claude Code のバージョンと、使うモデル・effort を [evaluation-sheet.md](evaluation-sheet.md) の「1. 実施条件」に書く（A と同じモデル・effort にする）
2. 開始時刻を書く（上限は 60 分）

## 1. 依頼する

1. このフォルダで Claude Code を **新しく** 起動する（設定を読み込ませるため）
2. `/aidlc` に続けて、[prompts/initial-request.md](../prompts/initial-request.md) の依頼文を **一字一句そのまま** 入力する

   ```text
   /aidlc IPv6 を前提とした VPC で、…（依頼文の全文）
   ```

   ワークフローの種類（Express など）は指定しない。AI-DLC が依頼内容から判断した結果を、そのまま記録する

## 2. 承認ゲートを進める

- 各ステージの最後に承認ゲートが表示される。内容を読み、**承認（Approve）** または **修正依頼（Request Changes）** を選ぶ
- 修正依頼をするときは、何をなぜ直してほしいかを文章で書く（監査ログに残り、ブログの「ブラッシュアップの実例」の根拠になる）
- **参考資料（既存のリポジトリ、URL、社内資料など）は渡さない**
- A の経験を踏まえて指摘した場面があれば、その場でメモしておく（評価シートの「4. 結果に影響しうる要因」に書く）

## 3. 終える

1. 60 分に達したら、途中でも終える。ワークフローの途中で止めた場合は、どのステージで止めたかを書く
2. Claude Code で `/export` を実行し、会話のログを `records/conversation-b.md` として保存する
3. 監査ログ（`aidlc/spaces/default/intents/*/audit/`）、成果物、会話ログに、アカウント ID、メールアドレス、端末のユーザー名やパスなどがないか確認し、あれば伏せ字にする
4. コミットして push する

   ```bash
   git add -A
   git status   # 認証情報や不要なファイルが含まれていないか確認する
   git commit -m "run: 検証 B を実施"
   git push -u origin run/b-YYYYMMDD
   ```

## 4. 評価する

[evaluation-sheet.md](evaluation-sheet.md) は、A・B の両方が終わってから記入します。
