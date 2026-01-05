# radio-browser-cli
Claudeのハルシネーションを現実にした,ウェブラジオを検索するCLIツール

- 参考

    - https://claude.ai/share/178464c2-800a-4e62-b46a-494cb54a96a3
    - https://claude.ai/public/artifacts/5c55cb4d-df3f-4b31-8520-4444f21a7708

## 概要
`radio-browser-cli`は、[Radio Browser](https://www.radio-browser.info/)のAPIを利用して、世界中のウェブラジオ局を検索するためのコマンドラインツールです。検索結果は、CSV、TSV、JSON、M3Uプレイリストなど、様々な形式で出力できます。

## APIとフェイルオーバー
このツールは、Radio Browser APIサーバーリストを `https://all.api.radio-browser.info/json/servers` にアクセスして動的に取得します。これにより、利用可能なサーバーを自動的に検出します。

取得に失敗した場合は、以下のフォールバックリストを使用します：
- `https://de1.api.radio-browser.info/json`
- `https://de2.api.radio-browser.info/json`
- `https://fi1.api.radio-browser.info/json`
- `https://fr1.api.radio-browser.info/json`
- `https://nl1.api.radio-browser.info/json`

APIリクエスト時は、取得したサーバーリストを順次試行するフェイルオーバー機能により、高い可用性を確保しています。

## 主な機能
- キーワード（局名）、国、タグ、言語によるラジオ局の検索
- 利用可能な国やタグの一覧表示
- 柔軟な出力形式（CSV, TSV, URLリスト, JSON, M3U）

## インストールと準備
### 1. 依存ライブラリのインストール
このツールは`requests`ライブラリを使用します。
```bash
pip install requests
```

### 2. スクリプトの配置
スクリプト (`radio-browser-cli`) をダウンロードし、実行権限を付与します。
```bash
chmod +x radio-browser-cli
```
どこからでもコマンドを実行できるように、スクリプトをPATHの通ったディレクトリ（例: `/usr/local/bin`）に移動またはシンボリックリンクを作成することを推奨します。
```bash
# 例
sudo mv radio-browser-cli /usr/local/bin/
```

### 3. 環境変数の設定（オプション）
API リクエストのタイムアウト時間を `RADIO_BROWSER_TIMEOUT` 環境変数でカスタマイズできます。デフォルトは10秒です。
```bash
export RADIO_BROWSER_TIMEOUT=30  # タイムアウトを30秒に設定
```

## 使い方

### ラジオ局を検索する (`search`)

**基本コマンド**
```bash
radio-browser-cli search [オプション]
```

**オプション**
- `--name NAME`: ラジオ局名で検索
- `--country COUNTRY`: 国名で検索 (例: "Japan")
- `--tag TAG`: ジャンル/タグで検索 (例: "jazz")
- `--language LANGUAGE`: 言語で検索 (例: "japanese")
- `--limit LIMIT`: 結果の最大表示数 (デフォルト: 20)
- `--format {csv,tsv,urls,json,m3u}`: 出力形式 (デフォルト: csv)

**使用例**
- **名前で検索 (例: "BBC")**
  ```bash
  radio-browser-cli search --name "BBC"
  ```

- **国とタグで検索し、M3U形式で出力**
  ```bash
  radio-browser-cli search --country "France" --tag "chanson" --limit 10 --format m3u
  ```

### 利用可能な国の一覧を表示する (`countries`)
```bash
radio-browser-cli countries
```
- `--limit LIMIT`: 表示数の上限を指定できます (デフォルト: 50)

### 利用可能なタグの一覧を表示する (`tags`)
```bash
radio-browser-cli tags
```
- `--limit LIMIT`: 表示数の上限を指定できます (デフォルト: 50)
