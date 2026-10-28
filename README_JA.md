<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="docs/assets/wordmark-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="docs/assets/wordmark-light.svg">
    <img src="docs/assets/wordmark-light.svg" alt="MiniMax Code" width="760">
  </picture>
</p>

<h1 align="center">MiniMax Code</h1>
<p align="center">プロンプトを、動くものに。MiniMax または自分のモデルで、ターミナルから構築・テストし、改善を続けましょう。</p>
<p align="center">
  <a href="#クイックスタート">Get started</a> ·
  <a href="docs/README.md">Documentation</a> ·
  <a href="docs/examples.md">Examples</a> ·
  <a href="CONTRIBUTING.md">Contributing</a>
</p>
<p align="center"><a href="README.md">English</a> · <a href="README_ZH.md">简体中文</a> · <strong>日本語</strong></p>
<p align="center">
  <img src="docs/assets/source-preview.svg" alt="Source preview">
  <img src="docs/assets/node.svg" alt="Compatibility: Node.js 22.19+, 24.2+, 25, and 26">
  <a href="LICENSE-STATUS.md"><img src="docs/assets/license.svg" alt="First-party default license: MIT"></a>
</p>

> [!NOTE]
> これは [README.md](README.md) の日本語訳です。内容が異なる場合は英語版が優先されます。

まばたきするポケットペットに集中タイマーを付けてみましょう。そして続けて「一時停止は長押しにして、タイマーが終わったらお祝いして」と頼んでみてください。リクエストが実際に使えるものに変わっていく様子を見られます。

[![Pocket Pet: まばたきする顔から、動く集中タイマーの相棒へ](docs/assets/pocket-pet-demo.png)](docs/demo.md)

<p align="center"><a href="docs/demo.md">構築の流れとブラウザデモを見る →</a> · <a href="examples/pocket-pet">自分で作ってみる →</a></p>

**ハードウェアは不要です。** このサンプルはブラウザ上でローカルに動作し、フロントエンドの依存関係もありません。CLI にコードを編集させるには、利用可能なクレジットのある MiniMax アカウント、または互換性のある独自のモデル API が必要です。モデルの呼び出しには料金が発生する場合があります。完成済みのサンプルはモデルアカウントなしで動作します。

## クイックスタート

### 1. MCode をインストールする

お使いのプラットフォーム向けの公式インストーラーを使用してください。最新の CLI をインストールし、必要に応じて互換性のある Node.js ランタイムを用意します。`sudo` や管理者権限は不要です。Alpine / musl Linux はワンコマンドインストーラーではサポートされていません。

**macOS / Linux / WSL**

```bash
curl -fsSL https://filecdn.minimax.chat/public/install.sh | bash
```

**Windows (PowerShell)**

```powershell
irm https://filecdn.minimax.chat/public/install.ps1 | iex
```

スクリプトは macOS / Linux / WSL では `~/.minimax-code`、Windows では `%USERPROFILE%\.minimax-code` にインストールします。ランチャーは POSIX では `bin/mcode` と `bin/mcode-tools`、Windows では `mcode.cmd` / `mcode.ps1` と `mcode-tools.cmd` / `mcode-tools.ps1` です。別の場所を指定するには、インストール前に `MCODE_INSTALL_DIR` を設定してください。CLI の削除については[アンインストール](#アンインストール)を参照してください。

**npm** — すでに **Node.js 22.19+ (22.x)、24.2+ (24.x)、25、または 26** がある場合:

```bash
npm install -g @minimax-ai/code@latest --registry=https://registry.npmjs.org/ --ignore-scripts=false --include=optional --allow-scripts=@minimax-ai/code,better-sqlite3
```

この npm コマンドは公開レジストリを使用し、オプションの SQLite 依存関係を含め、パッケージと SQLite のインストールスクリプトの実行を許可します。バージョンの固定や Node.js の互換性については[インストールガイド](docs/installation.md)を参照してください。

ターミナルを開き直して、インストールを確認します:

```bash
mcode --version
mcode --help
```

公式の[クイックスタート](https://agent.minimax.io/docs/cli/quick-start)、[機能](https://agent.minimax.io/docs/cli/features)、[トラブルシューティング](https://agent.minimax.io/docs/cli/faq)も参照してください。

### 2. サインインする、または自分の API キーを使う

中国本土アカウントの場合:

```bash
mcode login
```

グローバルアカウントの場合:

```bash
mcode login --region global
```

ブラウザでサインインを完了したら、`mcode` を開き、`/status` でアカウントを確認し、`/provider` でモデルを選択します。サインアウトするには `mcode logout` を実行します。

Token Plan には利用可能なクレジットのあるアカウントが必要です。このリポジトリからのビルドおよび公開済みの npm CLI `@minimax-ai/code@0.6.9` は、ユーザーデータの保存先としてデフォルトで `~/.minimax`(プロファイルを選択した場合は `~/.minimax-<profile>`)を使用します。`MINIMAX_DATA_DIR` または `MAVIS_DATA_DIR` でデータディレクトリを上書きできます。インストーラーのインストール先である `~/.minimax-code` はこれとは別です。設定やセッションの場所を確認・削除する前に、[アカウントとデータ](docs/installation.md#accounts-and-data)を参照してください。

<details>
<summary>自分の API キーを使う (BYOK)</summary>

BYOK では MiniMax へのログインは不要です。現在のシェルで `MCODE_PROVIDER_API_KEY` を設定してから、プロバイダーを追加します。サンプルの URL とモデル名は、お使いのプロバイダーの値に置き換えてください:

```bash
mcode provider add --name my-provider --base-url https://example.com/v1 \
  --api-format openai-completions --model my-model \
  --api-key-env MCODE_PROVIDER_API_KEY --use
mcode
```

`--use` を指定すると、保存して選択する前に、最初に指定したモデルをテストします。接続テストに失敗した場合は何も保存されません。テストせず、デフォルトモデルも変更せずに保存するには `--use` を省略します。カスタム/ローカルモデルの場合は `--context-limit 32768 --output-limit 4096` を追加してください(実際のサーバーの上限値を使用してください)。各値は正の安全な整数でなければならず、繰り返し指定したすべての `--model` に適用されます。設定済みの上限は `mcode provider list --json` で確認できます。これらのフラグを省略すると、既存のモデル上限のデフォルト値が維持されます。

サポートされている API フォーマット: `openai-completions`、`openai-responses`、`anthropic-messages`。環境変数の設定、接続確認、単一実行でのモデルの上書きについては[モデルの例](docs/examples.md#2-choose-your-own-model)を参照してください。

この方法で追加したプロバイダーは、有効なプロファイルの `config.yaml` の `custom_provider` に保存されます。これがサードパーティやセルフホストのエンドポイントに対してサポートされている方法で、`minimax_api` は公式の MiniMax API 専用です。Anthropic 互換エンドポイントで `Authorization: Bearer` を必要とするリレーでは、`config.yaml` でカスタムヘッダーを設定できます。詳しくは[サードパーティのリレーとカスタム認証ヘッダー](docs/examples.md#third-party-relays-and-custom-auth-headers)を参照してください。

</details>

### 3. ポケットペットを作る

このリポジトリをクローンし、スターターを別のディレクトリにコピーします:

```bash
git clone https://github.com/MiniMax-AI/minimax-code.git
cd minimax-code
node examples/pocket-pet/setup.mjs ../my-pocket-pet
cd ../my-pocket-pet
node serve.mjs
```

`http://127.0.0.1:4173` を開きます。2 つ目のターミナルで `my-pocket-pet` 内で `mcode` を開き、[最初のプロンプト](examples/pocket-pet/README.md#first-request)、続いて[追加のリクエスト](examples/pocket-pet/README.md#change-the-requirement)を貼り付けます。変更のたびにブラウザを更新してください。

先に完成形を試したい場合は、リポジトリのルートで `node examples/pocket-pet/serve.mjs finished` を実行し、同じ URL を開いてください。`?demo=1` を付けると、そのことが画面上に明示された 10 秒モードになります。

[詳しい手順と要件](examples/pocket-pet) · [小さなコード修正の例](examples/clamp) · [モデル、検索、ツール](docs/examples.md)


### 自分のプロジェクトで使う

作業したいプロジェクトを開きます:

```bash
cd /path/to/your/project
mcode
```

TUI でタスクを説明するか、MCode の起動時に直接タスクを渡します:

```bash
mcode "Find a failing test, fix the implementation, and run the relevant tests."
```

`mcode init .` を使うと、`AGENTS.md` にプロジェクトのガイダンスを生成または更新できます。期待する結果、許可する変更、タスクの検証方法を記述してください。

| エントリーポイント | コマンド | 用途 |
| --- | --- | --- |
| 対話型 TUI | `mcode [prompt]` | コードの探索、会話の継続、変更や権限のレビュー。 |
| ヘッドレス | `mcode exec [prompt]` | シェルスクリプト、CI、バッチ処理、評価。 |
| ACP | `mcode acp` | Agent Client Protocol をサポートするエディターやクライアント。 |

### 軽量な会話

簡単な会話や一般的な知識に関する質問には、`--mode lightweight` で新しいセッションを開始します:

```bash
mcode --mode lightweight "Explain how DNS caching works."
mcode exec --mode lightweight "Summarize the CAP theorem."
```

軽量モードでは、会話用の小さなシステムプロンプトのみを送信し、ツールスキーマは送信しません。ワークスペースの指示、Skills、メモリブロック、MCP スキーマ、完全な環境ブロックをプロバイダーのコンテキストから省くため、ローカルファイルを調べたり変更したりすることはできません。コーディングやツールを使う場合は、`mcode --mode standard` または `mcode exec --mode standard` で新しい標準セッションを開始してください。`--mode` を省略した場合は標準モードと同じです。

モードはルートセッションの作成時に固定されます。そのため `--mode lightweight` は `--session` や `--continue` と組み合わせられません。子セッション/サブエージェントのセッションはモードを継承せず、`/compact` は引き続き標準のコンパクションコンテキストを使用し、ACP とデスクトップのセッションは標準のままです。軽量セッションを開き直すとモードは維持され、TUI のステータスラインに `Lightweight` と表示されます。軽量モードで起動して標準セッションを開いた場合は標準のままで、インジケーターは表示されません。

### 作業を続ける

```bash
# 現在のワークスペースで最新のセッションを再開する
mcode --continue

# セッション選択画面を開く
mcode --session
```

TUI 内では、`/sessions` で過去のセッションを探し、`/help` ですべてのコマンドとショートカットを確認できます。

| 操作 | ショートカット |
| --- | --- |
| メッセージを送信する、または実行中のタスクの方向を修正する | `Enter` |
| タスクの実行中にフォローアップをキューに追加する | `Alt+Enter` |
| 改行を挿入する | `Shift+Enter` |
| ワークスペースのファイルやディレクトリを参照する | `@` |
| Plan Mode を切り替える | `Shift+Tab` |
| 権限モードを切り替える | `Alt+M` |
| パネルを閉じる、または実行中のタスクを中断する。モデルが応答する前に中断すると、メッセージが入力欄に戻る | `Esc` |

## アンインストール

アンインストールする前に、エディター統合を含む実行中の MCode セッションをすべて閉じてください。まず `command -v mcode`(macOS / Linux / WSL)または `Get-Command mcode -All`(PowerShell)でコマンドの場所を確認し、該当するインストール方法の手順に従ってください。現在の公式インストールスクリプトには、アンインストール用のフラグは**ありません**。

### スクリプトでインストールした場合

以下のコマンドは、両方のランチャー、ダウンロードしたリリース、インストーラーが管理する Node.js ランタイムを含む、デフォルトのインストールディレクトリを削除します。`MCODE_INSTALL_DIR` を使用した場合は、実際のインストールディレクトリに置き換えてください。削除前に中身を確認してください。以前のソースビルドでは `~/.minimax-code` をユーザーデータに使用しており、カスタムのデータディレクトリがインストール先と重なっている可能性もあります。残したい設定やセッションは、削除前にバックアップしてください。

**macOS / Linux / WSL**

```bash
rm -rf -- "$HOME/.minimax-code"
```

インストーラーが更新したシェルファイルから、`# MiniMax Code CLI` というコメントとそれに続く PATH の行を削除します。対象ファイルは、zsh では `~/.zshrc`、bash では `~/.bashrc`、`~/.bash_profile`、`~/.profile` のうち最初に存在するもの(または新規作成された `~/.bashrc`)、fish では `~/.config/fish/config.fish`、その他のシェルでは `~/.profile` です。その行は `export PATH="/absolute/install/path/bin:$PATH"`、fish の場合は `fish_add_path -g "/absolute/install/path/bin"` です。他の PATH 設定は残し、MCode のエントリーだけを削除してください。`MCODE_NO_MODIFY_PATH` が設定されている場合や、パスがすでに存在する場合、インストーラーはこの編集を行いません。

**Windows (PowerShell)**

```powershell
Remove-Item -LiteralPath "$env:USERPROFILE\.minimax-code" -Recurse -Force
```

**アカウントの環境変数を編集** を開き、ユーザーの **Path** を編集して、インストールディレクトリのエントリー(デフォルトでは `%USERPROFILE%\.minimax-code`。展開された絶対パスで表示される場合があります)だけを削除します。Windows のインストーラーは PowerShell プロファイルではなくユーザーの Path を更新します。`MCODE_NO_MODIFY_PATH` を設定すると、この永続的な更新は行われません。

### npm またはソースからインストールした場合

npm でグローバルにインストールした場合は、MCode のインストール時と同じ npm インストール/プレフィックスを使用してください:

```bash
npm uninstall -g @minimax-ai/code
```

ソースビルドの場合は、作業内容を保存したうえで、自分で作成したチェックアウトだけを削除してください。詳しくは[更新または削除](docs/installation.md#update-or-remove)を参照してください。

アンインストール後、ターミナルを開き直し(統合ターミナルの場合はエディターを完全に再起動し)、もう一度 `command -v mcode` または `Get-Command mcode -All` を実行します。何も表示されなければ、コマンドは PATH から削除されています。別のコピーが表示された場合は、そのインストール方法を確認してから削除してください。

### 任意: ユーザーデータを削除する

プログラムを削除しても、別に保存されているユーザーデータは残ります。ローカルのログイン状態、プロバイダー設定、キャッシュ、セッションも削除するには、まず[アカウントとデータ](docs/installation.md#accounts-and-data)で対象のディレクトリを確認し、必要なものをバックアップしてください。このディレクトリは他の MCode インストールと共有されている場合があります。デフォルトの `~/.minimax` ディレクトリの場合のみ:

```bash
# macOS / Linux / WSL — デフォルトのユーザーデータを完全に削除します
rm -rf -- "$HOME/.minimax"
```

```powershell
# Windows — デフォルトのユーザーデータを完全に削除します
Remove-Item -LiteralPath "$env:USERPROFILE\.minimax" -Recurse -Force
```

プロファイルは `~/.minimax-<profile>` を使用します。`MINIMAX_DATA_DIR` または `MAVIS_DATA_DIR` で別の場所を指定できます。ワイルドカードによる削除は使わず、破棄したいディレクトリだけを個別に削除してください。シェルプロファイルやユーザー環境設定に追加した MCode 用の環境変数が不要になった場合は、それらも削除してください。

## できること

| タスク | 機能 |
| --- | --- |
| **コードの編集と検証** | ファイルの読み取り、差分の確認、シェルコマンドやテストの実行、権限とサンドボックスによるツール実行の制御。 |
| **モデルの選択** | MiniMax アカウント / Token Plan、または OpenAI 互換・Anthropic 互換の API フォーマットを使うカスタムプロバイダー。 |
| **検索とメディアの活用** | 組み込みの検索、`mcode-tools` のメディアツール、MCP、マネージドコネクター(アカウントのアクセス権とサービスクレジットに依存)。 |
| **作業を前に進める** | セッションの再開、タスクの計画、サブエージェントの利用、公式・ローカル・GitHub のプラグインや組み込みスキルによるエージェントの拡張。 |
| **ワークフローとの連携** | ヘッドレス CLI でのスクリプト化されたタスクの実行、または ACP を介した互換エディターやクライアントとの接続。 |

アカウント機能、アップデート、フィードバック、診断機能も含まれています。マネージドツールにはネットワークアクセスと適切な認可が必要です。詳しくは[機能とサービスの境界](docs/tui-capabilities.md)を参照してください。


## ソースからビルドする

MCode を開発する、またはこのソースチェックアウトを実行するには、Git、**Node.js 22.19+ (22.x)、24.2+ (24.x)、25、または 26**、**pnpm 9.12.0** が必要です。Windows では、チェックアウトをローカルの NTFS ボリューム上に置き、クラウド同期フォルダーの外に配置してください。以下のプリフライトコマンドは、pnpm がワークスペースのリンクを作成する前にボリュームを確認します。
```bash
git clone https://github.com/MiniMax-AI/minimax-code.git
cd minimax-code
node scripts/check-windows-source-location.mjs
pnpm install --frozen-lockfile
pnpm build
pnpm mcode
```

初回のビルドにはインターネット接続が必要です。依存関係と整合性チェック済みの `mcode-tools` バンドルは公開 npm から取得されます。pnpm のセットアップ、システム依存関係、アップデートについては[ソースインストールガイド](docs/installation.md)を参照してください。

ソースディレクトリでは、上記の例の `mcode` の代わりに `pnpm mcode` を使用してください。自分のプロジェクトで作業するには、そのディレクトリを開いてビルド済みの CLI を起動します:

```bash
node /absolute/path/to/minimax-code/dist/cli.js
```

このリポジトリは **0.6.3 ソースプレビュー** を対象としています。公開パッケージのインストールとこのチェックアウトのビルドは別の経路です。バージョンが一致していても、ビルドの来歴が同一であることの証明にはなりません。詳しくは[バージョンとエビデンスのベースライン](docs/open-source-status.md#version-and-evidence-baseline)を参照してください。

## ドキュメントとコントリビューション

- [インストールとアップデート](docs/installation.md) · [サンプル](docs/examples.md) · [TUI ステータスライン](packages/tui/docs/status-line-config.md)
- [コントリビューターガイド](CONTRIBUTING.md) · [バグ報告やアイデアの提案](https://github.com/MiniMax-AI/minimax-code/issues/new/choose) · [セキュリティ問題の報告](SECURITY.md)
- [すべてのドキュメント](docs/README.md): アーキテクチャ、機能カバレッジ、検証記録、ソース同期、リリース準備。

ドキュメントの主要言語は英語です。このページは[英語版 README](README.md) の日本語訳です。

現在、コードとドキュメントのプルリクエストはリポジトリのコラボレーターからのみ受け付けています。コラボレーターではないものの、アイデアや提案がある場合は、[Issue を作成](https://github.com/MiniMax-AI/minimax-code/issues/new/choose)して議論させてください。報告からは、シークレット、アカウントの詳細、非公開のプロジェクト内容を取り除いてください。

## デスクトップアプリとサポート

<a href="https://agent.minimax.io/download" title="Download MiniMax Code">
  <img src="https://filecdn.minimax.chat/public/c3ebbd2e-f55b-48d7-adff-030abb63e06d.png" alt="MiniMax Code デスクトップアプリ — クリックしてダウンロード" width="100%" />
</a>

[macOS または Windows 版をダウンロード](https://agent.minimax.io/download) · [問題の報告や質問](https://github.com/MiniMax-AI/minimax-code/issues/new/choose)

このリポジトリでは、MiniMax Code デスクトップアプリの Issue 報告も受け付けています。公開されているソースはターミナル TUI、ヘッドレス CLI、ACP を対象としており、デスクトップアプリのソースは含まれていません。Issue を作成する際は、該当する製品を選択してください。デスクトップのバグの場合は、アプリのバージョン、オペレーティングシステム、および **Settings → General → Upload logs** から取得できる場合はログのアップロード ID を記載してください。CLI のバグの場合は、`mcode --version`、使用しているインターフェース、最小限の再現手順を記載してください。報告からは、認証情報と非公開のプロジェクト内容を取り除いてください。

## フィードバックと連絡先

| チャネル | 用途 |
| --- | --- |
| [GitHub Issues](https://github.com/MiniMax-AI/minimax-code/issues/new/choose) | CLI やデスクトップアプリに関する公開のバグ報告、機能リクエスト、質問。 |
| [MiniMaxCode@minimax.io](mailto:MiniMaxCode@minimax.io) | 一般的なフィードバックとサポートに関する問い合わせ。 |
| [security.mcode@minimax.io](mailto:security.mcode@minimax.io) | 非公開の脆弱性報告。再現の詳細と秘匿化した証拠をこちらに送ってください。詳しくは[セキュリティ](SECURITY.md)を参照してください。 |
| [Discord](https://minimax.io/discord) | コミュニティでの議論とフィードバック。 |
| [Feishu フィードバックグループの QR コード](https://cdn.hailuoai.com/hailuo-video-web/public_assets/minimax_code_feishu_group_url.png) | 中国語のコミュニティフィードバック。Feishu でスキャンするか、中国語版デスクトップアプリのユーザーメニュー → **Contact us → Feishu** から QR コードを確認してください。 |

最新情報は [X の MiniMax](https://x.com/MiniMaxAgent) をフォローしてください。脆弱性の詳細、認証情報、非公開のプロジェクト内容は、公開の Issue やコミュニティのチャットに載せないでください。

## ライセンス

ファーストパーティのコードはデフォルトで [MIT](LICENSE) ライセンスです。既存のファイル単位およびパッケージ単位のライセンスはそのまま維持されます。依存関係、アセット、`mcode-tools` については、[サードパーティに関する告知](THIRD_PARTY_NOTICES.md)と[ライセンスの状況](LICENSE-STATUS.md)を参照してください。
