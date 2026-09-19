# Upstream provenance

このディレクトリの内容は cloudflare/security-audit-skill からのベンダリング（取り込みコピー）である。

- 出典: https://github.com/cloudflare/security-audit-skill （`skills/security-audit/` 配下 + `LICENSE`）
- ライセンス: MIT（同梱の [LICENSE](LICENSE) を参照）
- 取込 commit: `c1c8a8c14710` (2026-09-14)

## ローカル差分

上流からの変更は `SKILL.md` の次の 2 箇所のみ。他の全ファイルはバイト単位で上流と同一に保つ。

1. frontmatter — `name: security`（`/jarvis:security` 用）と description への日本語トリガー追記
2. 本文冒頭の `<!-- jarvis-local:start -->` 〜 `<!-- jarvis-local:end -->` ブロック
   （引数マッピング、コスト承認ゲート、言語・ペルソナ分離、認可と成果物の扱い）

## 同期手順

1. 上流の対象 commit の tarball を取得する:
   `gh api repos/cloudflare/security-audit-skill/tarball/<commit> > skill.tar.gz`
2. 展開した `skills/security-audit/` と本ディレクトリを diff する。
   このとき `SKILL.md` は frontmatter と `jarvis-local` ブロックを除外して比較する
3. **差分を全件レビューする**（外部 URL・ネットワークアクセス・環境変数参照・
   ファイル書き込みの追加がないかを重点確認）。`main` 追随の自動更新は行わない
4. 問題なければ差分を適用し、`SKILL.md` のローカル 2 箇所を再適用する
5. バリデータのテストを実行して健全性を確認する:
   `node --test skills/security/validate-findings.test.cjs skills/security/validate-coverage-ledger.test.cjs`
6. 本ファイルの取込 commit を更新する
