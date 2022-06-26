## このドキュメントについて

### 本書の目的

当社ではソースコード管理ツールとして Git を、サービスとして GitHub を利用しています。

本書ではこれらの運用ルールを定めることで、開発効率の向上と持続可能なソースコード管理の実現を目指します。

### バージョン

Git は基本的に最新バージョンを用いるものとします。

ただし、特段の不都合がない場合は、随時更新しなくてもかまいません。

### Git のユーザー名とメールアドレス

社用の開発環境ではユーザー名とメールアドレスをグローバルに設定してください。

`user.name` はローマ字表記で `Kenji YAMADA` のように表現してください。
`user.email` は社用のメールアドレスか GitHub の `@users.noreply.github.com` のメールアドレスを設定してください。
(`@users.noreply.github.com` のメールアドレスは https://github.com/settings/emails で参照できます。)

```
git config --global user.name <ユーザー名>
git config --global user.email <メールアドレス>
```

## 目次

- [Git 管理ファイル](./config.md)
- [コミットルール](./commits.md)
- [Issue ルール](./issues.md)
- [ブランチルール](./branches.md)
- [Pull request ルール](./pull-requests.md)
