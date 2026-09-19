# jarvis

Claude Code を MCU（アイアンマン）の J.A.R.V.I.S. として振る舞わせるペルソナプラグイン。

ユーザーを「スターク」と呼び、冷静沈着な英国執事風の口調で応答しつつ、
先回りの進言・リスク指摘・選択肢の提示を行う。口調はあくまで演出で、
本体は「指示待ちにならない行動指示」の方にある。

## 構成

```
jarvis/
├── .claude-plugin/
│   ├── plugin.json          # プラグイン定義
│   └── marketplace.json     # マーケットプレイス定義（このリポジトリ単体で配布可能にする）
├── skills/
│   ├── jarvis/
│   │   └── SKILL.md         # /jarvis で発動するペルソナモード
│   ├── cognitive-rhythm-writing/
│   │   └── SKILL.md         # 日本語ライティング規範（ペルソナ発動中は常時考慮）
│   └── security/
│       ├── SKILL.md         # /jarvis:security で発動するセキュリティ監査
│       ├── UPSTREAM.md      # 出典・取込 commit・上流同期手順
│       └── ...              # 攻撃クラス別リファレンス 14 本 + バリデータ（上流原文のまま）
└── hooks/
    └── hooks.json.example   # SessionStart でペルソナを自動注入する hook（デフォルト無効）
```

## インストール

```
/plugin marketplace add N-i-ke/jarvis
/plugin install jarvis@jarvis
```

インストール後、`/jarvis` でペルソナモードが発動する。
「ジャービス」と呼びかけるだけでも Claude がスキルを拾って発動する。

## 常時オンにする（任意）

デフォルトでは `/jarvis` の手動発動のみ。セッション開始時に自動でペルソナを
注入したい場合は、hook ファイルをリネームして有効化する。

```
mv hooks/hooks.json.example hooks/hooks.json
```

JSON はコメントを書けないため、「コメントアウトで同梱」の代わりに
`.example` 拡張子で無効化してある。有効化後はプラグインの再インストール
（または Claude Code の再起動）で反映される。

## セキュリティ監査スキル（/jarvis:security）

[cloudflare/security-audit-skill](https://github.com/cloudflare/security-audit-skill)（MIT）を
ベンダリングしたもの。リポジトリの脆弱性を、偵察 → ハンティング → 反証検証 →
構造化出力 → 独立再検証 → レポート生成の 6 フェーズで網羅的に監査する。
出典と上流同期手順は `skills/security/UPSTREAM.md` を参照。

```
/jarvis:security                     # 対象と目的を確認してモード判定
/jarvis:security <質問や懸念>          # ガイダンスモード（成果物なし）
/jarvis:security audit [path]        # フル監査（standard プロファイル）
/jarvis:security quick [path]        # フル監査（quick プロファイル・軽量）
/jarvis:security client [path]       # クライアントサイド特化のスコープ付き監査
```

### 要件と注意

- バリデータの実行に Node.js が必要
- フル監査は多数のサブエージェントを起動し、相応のトークンを消費する。
  実行前にプロファイル・想定エージェント数・budget の承認を求める設計になっている。
  日常のレビューには `client` / `quick`、棚卸しには `audit` という使い分けを推奨
- 監査対象は自分が変更・監査権限を持つリポジトリに限ること
- 監査成果物（`~/security-audit-skill/<repo>/run-<N>/` 配下）は機密情報として扱い、
  リポジトリにコミットしない

## 開発メモ

- ペルソナの文面は `skills/jarvis/SKILL.md` が単一の情報源。hook もこのファイルを注入する
- コード・コミットメッセージ等の成果物にはペルソナを持ち込まない設計（SKILL.md 内で明示）
- `skills/cognitive-rhythm-writing/SKILL.md` は k16shikano 氏の認知リズム・ライティング規範を
  元に収録したもの（出典はファイル内に明記）。ペルソナ発動中は常に考慮される
