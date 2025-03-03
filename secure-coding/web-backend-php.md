# セキュアコーディング規定（バックエンド: Laravel / PHP）

## 1. 目的・適用範囲

本規定は、Laravel（PHP）を使用したバックエンド開発におけるセキュリティ対策を定め、情報漏洩や攻撃のリスクを最小限に抑えることを目的とする。

本規定は、[OWASP Top 10](https://owasp.org/www-project-top-ten/) のセキュリティリスクを考慮して作成されている。

## 2. 入力バリデーションとデータサニタイズ [OWASP A03:2021 - Injection]

### **2.1 入力バリデーションの徹底**

- `FormRequest` を使用して入力バリデーションを一元管理する。
- すべてのリクエストに対して型やフォーマットを検証する。

#### **✅ OK例（`FormRequest` を使用）**

```php
namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class StoreUserRequest extends FormRequest
{
    public function rules(): array
    {
        return [
            'email' => 'required|email|unique:users,email',
            'password' => 'required|min:8|confirmed',
        ];
    }
}
```

### **2.2 SQLインジェクション対策**

- `DB::raw()` を避け、Eloquent ORM または `query builder` を使用する。
- プレースホルダーを使用して SQL の組み立てを防ぐ。

#### **❌ NG例（SQLインジェクションのリスクあり）**

```php
$users = DB::select("SELECT * FROM users WHERE email = '$email'");
```

#### **✅ OK例（Eloquent ORM を使用）**

```php
$user = User::where('email', $email)->first();
```

## 3. 認証・認可 [OWASP A07:2021 - Identification and Authentication Failures]

### **3.1 Laravelの認証機能の適切な利用**

- `bcrypt()` を使用してパスワードを安全にハッシュ化する。
- `Auth` ファサードを利用し、ユーザーの認証を管理する。

#### **✅ OK例（パスワードのハッシュ化）**

```php
use Illuminate\Support\Facades\Hash;

$user->password = Hash::make('securepassword');
```

### **3.2 API認証（Sanctum の使用）**

- API 認証には `Laravel Sanctum` を使用し、Bearer トークンを活用する。
- トークンは `HttpOnly` クッキーで管理し、`localStorage` には保存しない。

#### **✅ OK例（Sanctumを利用したAPI認証）**

```php
use Laravel\Sanctum\Sanctum;

$token = $user->createToken('API Token')->plainTextToken;
return response()->json(['token' => $token]);
```

### **3.3 認可（ゲートとポリシーの活用）**

- `Gate` や `Policy` を使用し、適切なアクセス制御を行う。
- コントローラ内で直接ロールチェックを行わず、ポリシークラスで管理する。

#### **✅ OK例（ポリシーを利用した認可制御）**

```php
if (Gate::denies('update-post', $post)) {
    abort(403, 'Unauthorized');
}
```

## 4. エラーハンドリングとロギング [OWASP A09:2021 - Security Logging and Monitoring Failures]

### **4.1 エラーメッセージの適切な処理**

- `.env` の `APP_DEBUG` は本番環境では `false` に設定する。
- `try-catch` を適切に使用し、詳細なエラー情報をユーザーに表示しない。

#### **✅ OK例（本番環境でのエラー処理）**

```env
APP_DEBUG=false
```

### **4.2 ロギングの適切な管理**

- `storage/logs/` 内のログには機密情報（パスワード、トークン）を記録しない。
- `Monolog` を使用し、適切なログレベルを設定する。

#### **✅ OK例（ログ設定の変更）**

```php
use Illuminate\Support\Facades\Log;

Log::error('Unauthorized access detected', ['user_id' => $user->id]);
```

## 5. 環境変数とシークレット管理 [OWASP A06:2021 - Vulnerable and Outdated Components]

### **5.1 `.env` ファイルの管理**

- `.env` ファイルを **Git にコミットしない**。
- `.env.example` を用意し、必要な環境変数のみ記載する。

#### **✅ OK例（`.gitignore` に `.env` を追加）**

```plaintext
.env
.env.local
.env.production
```

### **5.2 APIキー・シークレットの管理**

- `.env` から直接フロントエンドにAPIキーを渡さない。
- 環境変数から値を取得する際は `config()` 関数を使用する。

#### **✅ OK例（環境変数の適切な使用）**

```php
$apiKey = config('services.api.key');
```

## 6. 依存ライブラリの管理 [OWASP A06:2021 - Vulnerable and Outdated Components]

### **6.1 `composer audit` による脆弱性チェック**

- `composer update` の前に `composer audit` を実行し、脆弱性のあるライブラリを確認する。

#### **✅ OK例（脆弱性の確認）**

```sh
composer audit
```

## 7. 参考資料

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Laravel Security Guide](https://laravel.com/docs/security)
- [PHP Security Best Practices](https://www.php.net/manual/en/security.php)
- [Monolog Documentation](https://seldaek.github.io/monolog/)
