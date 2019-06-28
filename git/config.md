## Git 管理ファイル

各リポジトリのルートディレクトリには .gitignore および .gitattributes を配置してください。

### .gitignore

.gitignore を配置することで、 Git 管理不要なファイルをコミットしてしまうことを防ぐことができます。

すでに類似した開発環境のリポジトリが存在する場合は、その設定ファイルを流用します。ただし、比較的新しい内容になっていることを確認してください。

新しい言語等の場合で適切なものが社内にない場合は、 [github/ignore](https://github.com/github/gitignore) から入手してください。

また標準的なファイルを除外するため、 [./defaults/.gitignore](./defaults/.gitignore) の内容が含まれていることを確認してください。


### .gitattributes

ドキュメントや大容量のログファイル、バイナリファイルが含まれる場合は Git LFS を有効にするため、 LFS 用の [.gitattributes](./defaults/.gitattributes) を配置してください。

GitHub での言語認識を適切にするため、必要に応じて linguist 属性 (https://github.com/github/linguist#overrides) を設定してください。

```
*.css linguist-vendored
*.scss linguist-vendored
*.js linguist-vendored
vendor/* linguist-vendored
```
