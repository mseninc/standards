# 権限管理（RBAC / ABAC）のセキュリティ規定

## 1. 目的・適用範囲

本規定は、システム内のユーザー権限管理を適切に実施し、
不正アクセスや権限の誤設定による情報漏洩を防ぐことを目的とする。

適用対象:
- **RBAC（ロールベースアクセス制御）**: 役割（例: Admin, User, Manager）に基づくアクセス制御
- **ABAC（属性ベースアクセス制御）**: ユーザーの属性（例: 部署、所属プロジェクト、デバイス種類）に基づくアクセス制御

本規定は、[NIST SP 800-162 (RBAC Guide)](https://csrc.nist.gov/publications/detail/sp/800-162/final) や [OWASP Access Control Guidelines](https://owasp.org/www-project-top-ten/) を考慮して作成されている。

## 2. RBAC（ロールベースアクセス制御）

### **2.1 RBAC の基本設計**

- **最小権限の原則**: デフォルトでは最小限の権限（read-only など）を付与し、必要に応じて昇格する。
- **役割（ロール）とリソースの対応を明確に定義する。**
- **RBAC は API, フロントエンド, DB すべてで一貫して適用する。**

### **2.2 RBAC の実装例（Next.js / Laravel）**

#### **✅ OK例（Next.js API Middleware で RBAC を適用）**

```typescript
export function authMiddleware(req: NextApiRequest, res: NextApiResponse, next: Function) {
  const token = req.cookies.token;
  if (!token) return res.status(401).json({ error: "Unauthorized" });

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    if (!decoded.roles.includes("admin")) {
      return res.status(403).json({ error: "Forbidden" });
    }
    req.user = decoded;
    next();
  } catch (err) {
    return res.status(403).json({ error: "Forbidden" });
  }
}
```

#### **✅ OK例（Laravel の Gate / Policy を利用した RBAC）**

```php
Gate::define('update-post', function ($user, $post) {
    return $user->role === 'admin' || $user->id === $post->user_id;
});
```

## 3. ABAC（属性ベースアクセス制御）

### **3.1 ABAC の基本設計**

- **アクセス制御をユーザーの属性やコンテキストに基づいて動的に決定する。**
- **条件ベースのアクセス制御ルールを明確に定義する。**
- **組織の階層（部署、チーム）、デバイスの種類、時間帯などを考慮する。**

### **3.2 ABAC の実装例（Next.js / Laravel）**

#### **✅ OK例（Next.js API Middleware で ABAC を適用）**

```typescript
export function abacMiddleware(req: NextApiRequest, res: NextApiResponse, next: Function) {
  const token = req.cookies.token;
  if (!token) return res.status(401).json({ error: "Unauthorized" });

  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    
    if (decoded.department !== "IT" || decoded.accessLevel < 3) {
      return res.status(403).json({ error: "Forbidden" });
    }
    req.user = decoded;
    next();
  } catch (err) {
    return res.status(403).json({ error: "Forbidden" });
  }
}
```

#### **✅ OK例（Laravel Policy で ABAC を適用）**

```php
public function view(User $user, Document $document)
{
    return $user->department === $document->department && $user->access_level >= 3;
}
```

## 4. API のエンドポイントとデータベースレベルのアクセス制御

### **4.1 API のエンドポイントごとのアクセス制御**

- **GET / POST / DELETE などの HTTP メソッドごとに適切な権限を設定する。**
- **認可チェックはミドルウェアまたはポリシーを通じて強制する。**

#### **✅ OK例（Next.js API でルートごとに権限制御）**

```typescript
export default function handler(req: NextApiRequest, res: NextApiResponse) {
  if (req.method === "DELETE" && req.user.role !== "admin") {
    return res.status(403).json({ error: "Forbidden" });
  }
  // 処理続行
}
```

### **4.2 データベースレベルでのアクセス制御**

- **ユーザーが取得できるデータを DB クエリレベルで制限する。**
- **SQL クエリや ORM の where 句で適切なフィルタリングを行う。**

#### **✅ OK例（Laravel Eloquent でアクセス制御）**

```php
$documents = Document::where('department', $user->department)->get();
```

## 5. ログと監査

### **5.1 重要な操作のログ記録**

- **管理者操作（ユーザー作成・削除、データ編集）をログに記録する。**
- **RBAC / ABAC の変更履歴を監査ログとして保持する。**

#### **✅ OK例（Laravel でログを記録）**

```php
Log::info('User role changed', ['user_id' => $user->id, 'new_role' => $newRole]);
```

### **5.2 アクセス制御ログの監視**

- **ログイン試行回数や API への不正アクセスを監視する。**
- **異常なアクセスがあった場合はアラートを発行する。**

#### **✅ OK例（Next.js API で不正アクセスログを記録）**

```typescript
if (req.method === "POST" && !req.user) {
  console.warn("Unauthorized access attempt", { ip: req.ip, endpoint: req.url });
}
```

## 6. 参考資料

- [NIST SP 800-162: Guide to Attribute-Based Access Control (ABAC)](https://csrc.nist.gov/publications/detail/sp/800-162/final)
- [OWASP Access Control Guidelines](https://owasp.org/www-project-top-ten/)
- [Laravel Authorization Gates & Policies](https://laravel.com/docs/authorization)
- [Next.js API Routes Middleware](https://nextjs.org/docs/api-routes/middleware)
