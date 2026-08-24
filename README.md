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
│   └── cognitive-rhythm-writing/
│       └── SKILL.md         # 日本語ライティング規範（ペルソナ発動中は常時考慮）
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

## 開発メモ

- ペルソナの文面は `skills/jarvis/SKILL.md` が単一の情報源。hook もこのファイルを注入する
- コード・コミットメッセージ等の成果物にはペルソナを持ち込まない設計（SKILL.md 内で明示）
- `skills/cognitive-rhythm-writing/SKILL.md` は k16shikano 氏の認知リズム・ライティング規範を
  元に収録したもの（出典はファイル内に明記）。ペルソナ発動中は常に考慮される
