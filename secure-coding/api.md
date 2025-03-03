# API セキュリティ規定

## 1. 目的・適用範囲

本規定は、フロントエンドおよびバックエンド間の API、外部 API 連携、Webhook などにおけるセキュリティ対策を定め、
情報漏洩や攻撃のリスクを最小限に抑えることを目的とする。

本規定は、[OWASP API Security Top 10](https://owasp.org/API-Security/) を考慮して作成されている。

## 2. 認証・認可 [OWASP API1:2023 - Broken Object Level Authorization]

### **2.1 API 認証の適切な実装**

- API には必ず認証を実装し、**認証なしのエンドポイントを最小限にする**。
- JWT (JSON Web Token) や OAuth2 を使用する場合、トークンの有効期限を適切に設定する。
- 認証トークンは `HttpOnly` Cookie に保存し、`localStorage` には保存しない。

#### **✅ OK例（JWT の適切な管理: Next.js, Laravel）**

```typescript
const token = jwt.sign({ userId: user.id }, process.env.JWT_SECRET, {
  expiresIn: "15m",
});

res.cookie("token", token, {
  httpOnly: true,
  secure: process.env.NODE_ENV === "production",
  sameSite: "Strict",
});
```

```php
$token = $user->createToken('API Token')->plainTextToken;
return response()->json(['token' => $token])->cookie(
    'token', $token, 15, '/', null, true, true
);
```

### **2.2 認可（アクセス制御）**

- `RBAC (ロールベースアクセス制御)` や `ABAC (属性ベースアクセス制御)` を実装する。
- API のパス (`/admin/*` など) に対して適切なアクセス権を設定する。
- `Gate` や `Policy` を使用し、エンドポイントごとに権限を制御する。

#### **✅ OK例（RBAC の実装: Next.js API Middleware）**

```typescript
export function authMiddleware(req: NextApiRequest, res: NextApiResponse, next: Function) {
  const token = req.cookies.token;
  if (!token) return res.status(401).json({ error: "Unauthorized" });
  
  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    if (decoded.role !== 'admin') {
      return res.status(403).json({ error: "Forbidden" });
    }
    req.user = decoded;
    next();
  } catch (err) {
    return res.status(403).json({ error: "Forbidden" });
  }
}
```

## 3. リクエスト・レスポンスの制御 [OWASP API3:2023 - Excessive Data Exposure]

### **3.1 CORS（Cross-Origin Resource Sharing）の適切な設定**

- `Access-Control-Allow-Origin: *` を避け、特定のオリジンのみ許可する。

#### **✅ OK例（許可するオリジンを制限: Laravel）**

```php
return response()
    ->json($data)
    ->header('Access-Control-Allow-Origin', 'https://example.com');
```

### **3.2 過剰なデータレスポンス防止**

- API のレスポンスデータは最小限にする。
- GraphQL では **フィールド制限** を適用し、不要なデータを取得させない。

#### **❌ NG例（過剰なレスポンス）**

```json
{
  "id": 1,
  "name": "User1",
  "password": "hashed_password",
  "credit_card": "1234-5678-9012-3456"
}
```

#### **✅ OK例（必要なデータのみ返す）**

```json
{
  "id": 1,
  "name": "User1"
}
```

### **3.3 レートリミットの適用（DDoS 攻撃対策）**

- API へのリクエスト数を制限し、異常なアクセスをブロックする。
- Laravel の `throttle` ミドルウェア、Next.js の `rate-limit` ライブラリを使用する。

#### **✅ OK例（Laravel のレートリミット）**

```php
Route::middleware(['throttle:60,1'])->group(function () {
    Route::get('/user', 'UserController@index');
});
```

## 4. Webhook のセキュリティ [OWASP API4:2023 - Lack of Resources & Rate Limiting]

### **4.1 Webhook の認証と署名検証**

- Webhook リクエストには **HMAC 署名** を使用し、リクエストの正当性を検証する。
- `X-Signature` ヘッダーを使用して署名を検証する。

#### **✅ OK例（Webhook の署名検証: Node.js）**

```typescript
import crypto from 'crypto';

function verifyWebhookSignature(req: Request) {
  const signature = req.headers["x-signature"];
  const hmac = crypto.createHmac("sha256", process.env.WEBHOOK_SECRET);
  hmac.update(JSON.stringify(req.body));
  const expectedSignature = hmac.digest("hex");
  return signature === expectedSignature;
}
```

## 5. 依存ライブラリの管理 [OWASP API6:2023 - Unrestricted Access to Sensitive Business Flows]

### **5.1 依存関係の脆弱性チェック**

- `npm audit`, `composer audit`, `dotnet list package --vulnerable` などを定期的に実行する。

#### **✅ OK例（脆弱性の確認）**

```sh
npm audit --json
composer audit
```

## 6. 参考資料

- [OWASP API Security Top 10](https://owasp.org/API-Security/)
- [Laravel Security Guide](https://laravel.com/docs/security)
- [Next.js Security Best Practices](https://nextjs.org/docs/security)
- [Node.js Security Best Practices](https://nodejs.org/en/docs/guides/security/)
