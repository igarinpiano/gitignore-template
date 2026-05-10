# gitignore-template

**ソースコード漏洩・認証情報の誤コミットを防ぐための `.gitignore` テンプレートです。**

近年、`.env`、`.map`、`CLAUDE.md`といったファイルや、秘密鍵・APIキーの誤コミットによる情報漏洩が後を絶ちません。このテンプレートはそうしたヒューマンエラーを防ぐための「最後の砦」として作られました。

---

## ⚠️ 重要：シークレットスキャンツールとの併用が必須です

`.gitignore` はあくまで **「コミットしないファイルを Git に伝える仕組み」** です。以下の点に注意してください。

- すでに一度コミットしたファイルは `.gitignore` に追加しても**除外されません**
- パターンマッチの抜け漏れは必ず発生します
- **シークレットスキャンツールを必ず組み合わせて使用してください**

| ツール | 用途 |
|---|---|
| [gitleaks](https://github.com/gitleaks/gitleaks) | コミット前・CI/CD でのシークレット検出 |
| [truffleHog](https://github.com/trufflesecurity/trufflehog) | Git 履歴全体のスキャン |
| [git-secrets](https://github.com/awslabs/git-secrets) | pre-commit フックによる予防 |
| [GitHub Secret Scanning](https://docs.github.com/en/code-security/secret-scanning) | GitHub 上での自動検出（無料） |

---

## 使い方

### 新規リポジトリ

```bash
curl -O https://raw.githubusercontent.com/igarinpiano/gitignore-template/main/.gitignore
```

### 既存リポジトリに追記

```bash
curl https://raw.githubusercontent.com/igarinpiano/gitignore-template/main/.gitignore >> .gitignore
```

### ホワイトリスト（必要なファイルを除外対象から外す）

プロジェクトによっては除外を解除したいファイルもあるかと思います。その場合は、`.gitignore` に `!` プレフィックスで追記してください。

```gitignore
# 例：サンプル用の .env ファイルはコミットしたい
!.env.example
!.env.sample

# 例：SQLのマイグレーションファイルはコミットしたい
!db/migrate/*.sql
```

---

## カバー範囲

| カテゴリ | 主な対象 |
|---|---|
| 環境変数・dotenv | `.env.*`、`.envrc` 等 |
| 証明書・秘密鍵 | `*.pem`、`*.key`、`*.p12`、SSH キー、GPG キー 等 |
| クラウド認証情報 | AWS / Azure / GCP / OCI / Alibaba Cloud 等 |
| パスワードマネージャー | `.vault-token`、`*.kdbx`、Bitwarden CLI 等 |
| フレームワーク固有 | Rails credentials、Django `local_settings.py`、WordPress `wp-config.php` 等 |
| AI エディタ・エージェント | `.claude/`、`.cursor/`、`.aider*`、`.copilot/` 等 |
| シェル・REPL 履歴 | `.bash_history`、`.zsh_history`、各種 REPL 履歴 |
| インフラ (IaC) | Terraform / Terragrunt / Pulumi 等の state・変数ファイル |
| コンテナ・K8s | Docker Compose オーバーライド、Helm values、Secret マニフェスト 等 |
| データベース | DB ダンプ、物理ファイル、GUI クライアント接続設定 等 |
| セキュリティスキャン結果 | `*.sarif`、gitleaks / trivy レポート 等 |
| E2E テスト | Playwright / Cypress のセッション・Cookie 等 |
| ヒューマンエラー対策 | メモファイル、バックアップ残骸、スクリーンショット 等 |
| OS / エディタ | macOS / Windows / Linux メタデータ、主要 IDE 設定 |
| 言語・フレームワーク | Node.js、Python、Swift、Ruby、Go、PHP、.NET 等 |

---

## 注意点

- このテンプレートは**幅広いプロジェクトを想定した保守的な設定**です。プロジェクトの要件に応じてホワイトリストを活用してください。
- `*.conf`、`*.yaml` など汎用的な拡張子の一部はコメントに注意事項を記載しています。フレームワークの設定ファイルと衝突しないかを確認してください。
- Apache License 2.0 のもとで提供されています。詳細は [LICENSE](./LICENSE) を参照してください。

---

## Contributing

本テンプレートにはまだ網羅できていないケースもあるかと思います。皆様のPull Requestsをお待ちしております。

新しい漏洩パターンの発見、ツール・フレームワーク固有の設定ファイルの追加、既存パターンの改善など、どんな改善でもお気軽にどうぞ。

- 追加するパターンには、**なぜ除外すべきか**を簡潔にコメントで添えてください
- 既存のセクション構造に合わせて配置してください
- 重複がないか確認してください（`grep` で確認できます）

---

## 関連リソース

- [gitignore.io](https://www.toptal.com/developers/gitignore) — 言語・OS 別テンプレート生成
- [GitHub の公式 gitignore コレクション](https://github.com/github/gitignore)
- [OWASP — Sensitive Data Exposure](https://owasp.org/www-project-top-ten/)

---

## 終わりに

昨今、AIコーディングツールの利用が加速しており、人的ミスによる情報漏洩のニュースもよく耳にします。\
情報には残存性・複製性・伝播性があり、一度公開された情報は、コピー・保存・拡散され続けます。デジタルタトゥーという言葉の通り、一度公開して拡散されてしまえば何をしても後の祭です。完全に取り戻すのは極めて困難、あるいは不可能と言えるでしょう。\
「あの時確認しておけば…」という後悔をしないためにも、セーフティネットとして、ぜひこの`.gitignore`をお役立てください。\
今後、新たなコーディングツールの登場や発展に伴って、`.gitignore`の内容を書き換える必要が出てくることもあるかと思います。その時はぜひお知らせください。\
欠陥の発見や追加すべきファイルの提案など、皆様の Pull Requests をお待ちしております。\
\
この`.gitignore`によって、誰か一人のミスでも未然に防ぐことができたならば、製作者としてこれ以上の喜びはありません。\
\
Igarin
