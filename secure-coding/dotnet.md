# セキュアコーディング規定（バックエンド: .NET / C#）

## 1. 目的・適用範囲

本規定は、.NET（C#）を使用したバックエンド開発におけるセキュリティ対策を定め、情報漏洩や攻撃のリスクを最小限に抑えることを目的とする。

本規定は、[OWASP Top 10](https://owasp.org/www-project-top-ten/) のセキュリティリスクを考慮して作成されている。

## 2. 入力バリデーションとデータサニタイズ [OWASP A03:2021 - Injection]

### **2.1 入力バリデーションの徹底**

- `DataAnnotations` または `FluentValidation` を使用し、入力値の型・フォーマットを検証する。
- API のリクエストボディ、クエリパラメータ、ヘッダー、URL パスに対してバリデーションを必ず実施する。

#### **✅ OK例（`DataAnnotations` を使用したバリデーション）**

```csharp
using System.ComponentModel.DataAnnotations;

public class UserModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }
    
    [Required]
    [MinLength(8)]
    public string Password { get; set; }
}
```

### **2.2 SQLインジェクション対策**

- `SqlCommand` ではなく、`SqlParameter` を使用する。
- ORM（Entity Framework, Dapper など）を推奨する。

#### **❌ NG例（SQLインジェクションのリスクあり）**

```csharp
string query = $"SELECT * FROM Users WHERE Email = '{email}'";
SqlCommand command = new SqlCommand(query, connection);
```

#### **✅ OK例（パラメータ化クエリを使用）**

```csharp
string query = "SELECT * FROM Users WHERE Email = @Email";
SqlCommand command = new SqlCommand(query, connection);
command.Parameters.AddWithValue("@Email", email);
```

## 3. 認証・認可 [OWASP A07:2021 - Identification and Authentication Failures]

### **3.1 認証の適切な実装**

- `ASP.NET Identity` を使用し、パスワードを `PBKDF2` でハッシュ化する。
- `JwtBearer` 認証を利用し、トークンを `HttpOnly Cookie` に保存する。

#### **✅ OK例（ASP.NET Identity を使用）**

```csharp
using Microsoft.AspNetCore.Identity;

var hashedPassword = new PasswordHasher<string>().HashPassword(null, "securepassword");
```

### **3.2 認可（ロールベースアクセス制御: RBAC）**

- `Authorize` 属性を活用し、適切なアクセス制御を実施する。

#### **✅ OK例（RBAC の実装）**

```csharp
[Authorize(Roles = "Admin")]
public IActionResult AdminDashboard()
{
    return View();
}
```

## 4. エラーハンドリングとロギング [OWASP A09:2021 - Security Logging and Monitoring Failures]

### **4.1 エラーメッセージの適切な処理**

- `UseExceptionHandler` を使用し、詳細なエラーメッセージを表示しない。

#### **✅ OK例（カスタムエラーハンドリング）**

```csharp
app.UseExceptionHandler("/Home/Error");
```

### **4.2 ロギングの適切な管理**

- `Serilog` または `NLog` を使用し、適切なログ管理を行う。
- 機密情報（パスワード、トークン）はログに記録しない。

#### **✅ OK例（Serilog の設定）**

```csharp
Log.Logger = new LoggerConfiguration()
    .WriteTo.Console()
    .WriteTo.File("logs/log.txt", rollingInterval: RollingInterval.Day)
    .CreateLogger();
```

## 5. 環境変数とシークレット管理 [OWASP A06:2021 - Vulnerable and Outdated Components]

### **5.1 `appsettings.json` の管理**

- 機密情報は `appsettings.json` に直接記述せず、`User Secrets` または `Azure Key Vault` を使用する。

#### **✅ OK例（環境変数から取得）**

```csharp
var connectionString = Environment.GetEnvironmentVariable("DB_CONNECTION_STRING");
```

### **5.2 APIキー・シークレットの管理**

- `.env` ファイルまたは `Azure Key Vault` を使用して管理する。

#### **✅ OK例（`dotnet user-secrets` を使用）**

```sh
dotnet user-secrets set "API_KEY" "your-secret-key"
```

## 6. 依存ライブラリの管理 [OWASP A06:2021 - Vulnerable and Outdated Components]

### **6.1 `dotnet list package --vulnerable` による脆弱性チェック**

- `dotnet list package --vulnerable` を定期的に実行し、脆弱性のあるライブラリを確認する。

#### **✅ OK例（脆弱性の確認）**

```sh
dotnet list package --vulnerable
```

## 7. 参考資料

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [ASP.NET Security Best Practices](https://docs.microsoft.com/en-us/aspnet/core/security/)
- [Serilog Documentation](https://serilog.net/)
- [NLog Documentation](https://nlog-project.org/)
