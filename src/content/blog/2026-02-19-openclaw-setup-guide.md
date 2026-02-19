---
title: '【2026年最新】OpenClawの始め方｜AIアシスタントを自分専用にする完全ガイド'
description: '初心者でも安心！OpenClawを使って自分だけのAIアシスタントを構築する手順を、セキュリティを考慮しながら丁寧に解説します。Discord・Telegram対応、無料で始められます。'
pubDate: '2026-02-19'
heroImage: '../../assets/thumbnails/2026-02-19-openclaw-setup-guide.png'
icons: ['settings', 'terminal', 'ai-brain']
tags: ['OpenClaw', 'AI', 'チャットボット', '自動化', 'セキュリティ']
---

## はじめに

「ChatGPTやClaudeを、自分のSlackやDiscordで使えたら便利なのに...」

そう思ったことはありませんか？**OpenClaw**を使えば、それが実現できます。

OpenClawは、AIアシスタントを自分のデバイスで動かし、普段使っているメッセージアプリ（Discord、Telegram、Slack、WhatsAppなど）から操作できるオープンソースツールです。

この記事では、**セキュリティを重視しながら**、初心者の方でも迷わずにOpenClawを設定できるように解説します。

## この記事で得られること

- OpenClawとは何か、何ができるのかを理解できる
- 安全にOpenClawをインストール・設定する手順がわかる
- Discordで自分専用のAIアシスタントを使えるようになる

## 必要なもの（事前準備）

OpenClawを始める前に、以下を準備してください。

### 必須

| 項目 | 説明 |
|------|------|
| **PC** | macOS、Linux、またはWindows（WSL2推奨） |
| **Node.js v22以上** | [公式サイト](https://nodejs.org/)からインストール |
| **LLM APIキーまたはサブスクリプション** | Anthropic Claude Pro/Max（推奨）またはOpenAI ChatGPT Plus |
| **メッセージアプリ** | Discord、Telegram、Slackなど |

### 費用について

- **OpenClaw本体**: 無料（オープンソース）
- **LLM利用料**: 月額約$20〜（Claude Pro/Max、ChatGPT Plusなど）

> 💡 **ポイント**: OpenClawは「ガワ」です。AIの頭脳部分（LLM）は別途必要になります。Anthropic Claude Pro/MaxかOpenAI ChatGPT Plusのサブスクリプションがあれば、追加のAPIキー購入なしでOAuth認証で使えます。

## インストール手順

### ステップ1: Node.jsの確認

まず、Node.jsがインストールされているか確認します。

```bash
node -v
# v22.x.x と表示されればOK
```

バージョンが古い場合は、[Node.js公式サイト](https://nodejs.org/)からv22以上をインストールしてください。

### ステップ2: OpenClawのインストール

ターミナルで以下のコマンドを実行します。

```bash
npm install -g openclaw@latest
```

インストールが完了したら、バージョンを確認します。

```bash
openclaw --version
```

### ステップ3: オンボーディングウィザードの実行

OpenClawには、設定を対話形式で進められるウィザードがあります。

```bash
openclaw onboard --install-daemon
```

ウィザードが以下を案内してくれます：

1. **LLM認証**: AnthropicまたはOpenAIへのログイン
2. **ワークスペース設定**: 作業ディレクトリの作成
3. **チャンネル設定**: Discord/Telegram/Slackの接続
4. **デーモン設定**: バックグラウンド起動の設定

> 🔐 **セキュリティTip**: ウィザードで設定した認証情報は `~/.openclaw/` に保存されます。このディレクトリのパーミッションは `700` に設定されており、他のユーザーからはアクセスできません。

## セキュリティを重視した設定

### DMポリシーの設定（重要）

OpenClawはデフォルトで**pairing**モードになっています。これは知らない人からのDMを自動でブロックする、安全な設定です。

```json
{
  "channels": {
    "discord": {
      "dmPolicy": "pairing"
    }
  }
}
```

**各モードの説明:**

| モード | 動作 | 推奨度 |
|--------|------|--------|
| `pairing` | 未登録ユーザーにはペアリングコードを表示、承認後のみ応答 | ⭐⭐⭐ |
| `open` | すべてのDMに応答（危険） | ❌ |

### allowFromの設定

自分だけが使えるように、ユーザーIDを明示的に指定します。

```json
{
  "channels": {
    "discord": {
      "dmPolicy": "pairing",
      "allowFrom": ["あなたのDiscordユーザーID"]
    }
  }
}
```

DiscordユーザーIDの確認方法：
1. Discordの設定 → 詳細設定 → 開発者モードをON
2. 自分のアイコンを右クリック → 「IDをコピー」

### 設定の健全性チェック

設定が安全かどうかを確認するコマンドがあります。

```bash
openclaw doctor
```

このコマンドは、危険な設定（DMポリシーがopenになっているなど）を警告してくれます。

## Discordボットの設定

最も簡単に始められるDiscordの設定方法を紹介します。

### ステップ1: Discordアプリケーション作成

1. [Discord Developer Portal](https://discord.com/developers/applications) を開く
2. 「New Application」をクリック
3. 名前を入力（例: My AI Assistant）

### ステップ2: Bot設定

1. 左メニューの「Bot」を選択
2. 「Add Bot」をクリック
3. **Botトークン**をコピー（後で使います）
4. 「MESSAGE CONTENT INTENT」を**ON**にする（重要！）

### ステップ3: Botを招待

1. 左メニューの「OAuth2」→「URL Generator」
2. Scopesで `bot` を選択
3. Bot Permissionsで以下を選択:
   - Send Messages
   - Read Message History
   - Attach Files
4. 生成されたURLをブラウザで開き、サーバーに招待

### ステップ4: OpenClawに設定

`~/.openclaw/openclaw.json` を編集（なければ作成）:

```json
{
  "agent": {
    "model": "anthropic/claude-opus-4-6"
  },
  "channels": {
    "discord": {
      "enabled": true,
      "token": "あなたのBotトークン",
      "dmPolicy": "pairing",
      "allowFrom": ["あなたのDiscordユーザーID"]
    }
  }
}
```

### ステップ5: Gateway起動

```bash
openclaw gateway restart
```

これでDiscordでボットにメンションすれば、AIが応答するようになります！

## 動作確認

Discordで以下を試してみましょう：

```
@あなたのBot こんにちは！
```

AIアシスタントが応答すれば成功です。

### よく使うコマンド

| コマンド | 説明 |
|----------|------|
| `/status` | 現在のセッション状態を表示 |
| `/new` または `/reset` | セッションをリセット |
| `/model sonnet` | モデルを切り替え（事前設定が必要） |

## トラブルシューティング

### Botが応答しない

1. **MESSAGE CONTENT INTENT** がONになっているか確認
2. `openclaw gateway status` でGatewayが動作しているか確認
3. `openclaw doctor` で設定エラーがないか確認

### 認証エラーが出る

```bash
# 認証をやり直す
openclaw auth login
```

### ログを確認する

```bash
openclaw gateway logs
```

## 次のステップ

OpenClawを設定できたら、以下にも挑戦してみましょう：

- **Telegramの追加**: `openclaw onboard` で追加可能
- **Voice Wake（音声起動）**: macOSアプリで「Hey Claude」的な体験
- **Skills（スキル）**: カスタムツールでOpenClawを拡張
- **Cron Jobs**: 定期実行タスクの設定

## まとめ

OpenClawを使えば、AIアシスタントを自分のデバイスで動かし、セキュアに運用できます。

**ポイントのおさらい:**

- ✅ `dmPolicy: "pairing"` で不正アクセスを防止
- ✅ `allowFrom` で許可ユーザーを明示
- ✅ `openclaw doctor` で設定を定期チェック
- ✅ LLMはAnthropic Claude Pro/Maxが推奨

セキュリティを意識しながら、自分だけのAIアシスタントを楽しんでください！

---

**参考リンク:**
- [OpenClaw公式ドキュメント](https://docs.openclaw.ai)
- [OpenClaw GitHub](https://github.com/openclaw/openclaw)
- [OpenClaw Discord](https://discord.gg/clawd)
