# インフラ・環境設定のセキュリティ規定

## 1. 目的・適用範囲

本規定は、弊社のインフラ環境におけるセキュリティ対策を定め、
情報漏洩や不正アクセスのリスクを最小限に抑えることを目的とする。

適用対象:
- **IaC（Infrastructure as Code）**: Ansible
- **CI/CD**: GitHub Actions, Jenkins
- **クラウド・サーバー環境**: Linux サーバー、Docker、環境変数管理

本規定は、[OWASP DevSecOps Guidelines](https://owasp.org/www-project-devsecops-guideline/) を考慮して作成されている。

## 2. サーバーおよびネットワークのセキュリティ設定

### **2.1 SSH 設定の強化**

- `root` ユーザーの SSH ログインを禁止する。
- SSH 接続は **公開鍵認証を必須** とし、パスワード認証を無効化する。
- SSH ポートは **デフォルトの `22` 以外に変更** し、`fail2ban` などでブルートフォース攻撃を防ぐ。

#### **✅ OK例（Ansible で SSH 設定を適用）**

```yaml
- name: Secure SSH Configuration
  lineinfile:
    path: /etc/ssh/sshd_config
    regexp: '^#?PermitRootLogin'
    line: 'PermitRootLogin no'
  notify: Restart SSH
```

### **2.2 ファイアウォール設定**

- `ufw`（Ubuntu）または `firewalld`（RHEL系）を有効化し、最小限のポートのみ開放する。
- **SSH は特定の IP のみに許可** し、全開放 (`0.0.0.0/0`) しない。

#### **✅ OK例（Ansible で UFW 設定を適用）**

```yaml
- name: Configure UFW
  ufw:
    rule: allow
    port: '22'
    proto: tcp
    src: '192.168.1.0/24'
```

## 3. 環境変数とシークレット管理

### **3.1 `.env` ファイルの管理**

- `.env` ファイルを **Git にコミットしない**。
- `.env.example` を用意し、必要な環境変数のみ記載する。

#### **✅ OK例（`.gitignore` に `.env` を追加）**

```plaintext
.env
.env.local
.env.production
```

### **3.2 GitHub Actions のシークレット管理**

- **機密情報（APIキー、DB接続情報）は `GitHub Secrets` に格納** し、ワークフロー内で直接記述しない。

#### **✅ OK例（GitHub Actions の Secrets 利用）**

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Set up environment
        run: echo "DB_PASSWORD=${{ secrets.DB_PASSWORD }}" >> .env
```

### **3.3 Jenkins のシークレット管理**

- **Jenkins の `Credentials Plugin` を使用し、パスワードや API キーを管理する。**
- `.env` などの環境変数を直接 Jenkinsfile に記述しない。

#### **✅ OK例（Jenkinsfile でシークレットを使用）**

```groovy
pipeline {
    environment {
        DB_PASSWORD = credentials('db-password')
    }
    stages {
        stage('Deploy') {
            steps {
                sh 'echo "DB_PASSWORD=$DB_PASSWORD" > .env'
            }
        }
    }
}
```

## 4. CI/CD のセキュリティ対策

### **4.1 GitHub Actions のセキュリティ強化**

- **ワークフローを PR（プルリクエスト）イベントで自動実行しない**（意図しないコード実行を防ぐ）。
- `permissions` を **最小権限の原則に従って設定** し、不要なリソースへのアクセスを制限する。

#### **✅ OK例（GitHub Actions の権限制限）**

```yaml
jobs:
  deploy:
    permissions:
      contents: read
      id-token: write
```

### **4.2 Jenkins のセキュリティ強化**

- **Jenkins の管理者権限を最小限にする**。
- `Pipeline: Groovy Sandbox` を有効にし、危険なスクリプトの実行を防ぐ。

#### **✅ OK例（Jenkinsfile の Sandbox 制約）**

```groovy
properties([
    pipelineTriggers([
        cron('H 0 * * *')
    ])
])
```

## 5. Docker / コンテナのセキュリティ [OWASP Docker Security]

### **5.1 ルートユーザーの使用禁止**

- Docker コンテナ内で `root` ユーザーを使用しない。
- `USER` 指定を行い、特権のないユーザーで実行する。

#### **✅ OK例（Dockerfile で非 root ユーザーを使用）**

```dockerfile
FROM node:18-alpine
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
```

### **5.2 イメージの脆弱性スキャン**

- `Trivy`, `Docker Scout` などのツールで定期的に脆弱性チェックを行う。

#### **✅ OK例（Trivy で脆弱性スキャン）**

```sh
trivy image my-app:latest
```

## 6. ログ管理と監視

### **6.1 ログの適切な管理**

- 機密情報（パスワード、トークン）をログに記録しない。
- ログの保存期間を適切に設定し、定期的にアーカイブする。

### **6.2 監視とアラートの導入**

- **`Prometheus + Grafana`、`AWS CloudWatch` などを活用し、異常なアクティビティを検知する。**
- **ログイン試行回数、API エラー率の監視を行い、アラートを設定する。**

## 7. 参考資料

- [OWASP DevSecOps Guidelines](https://owasp.org/www-project-devsecops-guideline/)
- [Ansible Security Best Practices](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html)
- [GitHub Actions Security Best Practices](https://securitylab.github.com/research/github-actions-security-best-practices/)
- [Jenkins Security Guidelines](https://www.jenkins.io/doc/book/system-administration/security/)
