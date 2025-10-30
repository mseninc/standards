# セキュアコーディング規定（バックエンド: Next.js / Node.js）

## 1. 目的・適用範囲

本規定は、Next.js（Node.js）を使用したバックエンド開発におけるセキュリティ対策を定め、情報漏洩や攻撃のリスクを最小限に抑えることを目的とする。

本規定は、[OWASP Top 10](https://owasp.org/www-project-top-ten/) のセキュリティリスクを考慮して作成されている。

## 2. 入力バリデーションとデータサニタイズ [OWASP A03:2021 - Injection]

### **2.1 入力バリデーションの徹底**

- `zod` や `yup` などのライブラリを使用し、入力値の型・フォーマットを検証する。
- APIのリクエストボディ、クエリパラメータ、ヘッダー、URLパスに対してバリデーションを必ず実施する。

#### **✅ OK例（`zod` を使用したバリデーション）**

```typescript
import { z } from "zod";

const userSchema = z.object({
  email: z.string().email(),
  password: z.string().min(8),
});

export function validateUserInput(input: any) {
  return userSchema.safeParse(input);
}
```

### **2.2 SQLインジェクション対策**

- ORM（Prisma, Sequelize）またはプリペアドステートメントを使用する。
- `query` メソッドで直接SQL文字列を扱わない。

#### **❌ NG例（SQLインジェクションのリスクあり）**

```typescript
const result = await db.query(`SELECT * FROM users WHERE email = '${email}'`);
```

#### **✅ OK例（Prisma を使用した安全な方法）**

```typescript
const user = await prisma.user.findUnique({ where: { email } });
```

## 3. 認証・認可 [OWASP A07:2021 - Identification and Authentication Failures]

### **3.1 認証の適切な実装**

- JWT は **`HttpOnly` Cookie** に保存し、`localStorage` は使用しない。
- `jsonwebtoken` の `expiresIn` を適切に設定し、短命なアクセストークンを使用する。
- リフレッシュトークンを適切に管理し、不正利用を防ぐ。

#### **✅ OK例（JWT の適切な管理）**

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

### **3.2 認可（アクセス制御）**

- APIルートは `middleware` を使用し、適切な認可チェックを実施する。
- RBAC（ロールベースアクセス制御）または ABAC（属性ベースアクセス制御）を実装する。

#### **✅ OK例（ミドルウェアで認可チェック）**

```typescript
export function authMiddleware(req: NextApiRequest, res: NextApiResponse, next: Function) {
  const token = req.cookies.token;
  if (!token) return res.status(401).json({ error: "Unauthorized" });
  
  try {
    const decoded = jwt.verify(token, process.env.JWT_SECRET);
    req.user = decoded;
    next();
  } catch (err) {
    return res.status(403).json({ error: "Forbidden" });
  }
}
```

## 4. エラーハンドリングとロギング [OWASP A09:2021 - Security Logging and Monitoring Failures]

### **4.1 エラーメッセージの適切な処理**

- ユーザー向けのエラーメッセージには、システム内部情報を含めない。
- `console.log()` は本番環境では使用せず、適切なロギングライブラリ（`winston`, `pino` など）を使用する。

#### **❌ NG例（詳細なエラー情報をクライアントに返す）**

```typescript
app.get("/user", async (req, res) => {
  try {
    const user = await getUser(req.userId);
    res.json(user);
  } catch (error) {
    res.status(500).json({ message: error.message });
  }
});
```

#### **✅ OK例（安全なエラーハンドリング）**

```typescript
app.get("/user", async (req, res) => {
  try {
    const user = await getUser(req.userId);
    res.json(user);
  } catch (error) {
    console.error("Error fetching user", error);
    res.status(500).json({ message: "Internal Server Error" });
  }
});
```

## 5. 環境変数とシークレット管理 [OWASP A06:2021 - Vulnerable and Outdated Components]

### **5.1 `.env` ファイルの管理**

- `.env` ファイルを **Git にコミットしない**。
- `dotenv` を使用して環境変数を適切に管理する。

#### **✅ OK例（`.env` の活用）**

```plaintext
JWT_SECRET=supersecretkey
DATABASE_URL=postgres://user:password@localhost:5432/dbname
```

### **5.2 APIキー・シークレットの管理**

- APIキーは `.env` ファイルまたは **シークレット管理ツール（AWS Secrets Manager, Vault, Doppler など）** を使用する。
- `.env` から直接フロントエンドにAPIキーを渡さない。

## 6. 参考資料

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Next.js Security Best Practices](https://nextjs.org/docs/security)
- [Node.js Security Best Practices](https://nodejs.org/en/docs/guides/security/)
- [Prisma Security Guide](https://www.prisma.io/docs/concepts/security)
