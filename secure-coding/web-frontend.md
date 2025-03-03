# セキュアコーディング規定（フロントエンド: TypeScript / JavaScript）

## 1. 目的・適用範囲

本規定は、Webアプリケーションのフロントエンド（ブラウザ側）におけるセキュアコーディングの指針を定め、情報漏洩や攻撃のリスクを最小限に抑えることを目的とする。

適用対象は、TypeScript / JavaScript を使用するフロントエンド開発であり、React, Vue, Angular などのフレームワークを含む。

本規定は、[OWASP Top 10](https://owasp.org/www-project-top-ten/) のセキュリティリスクを考慮して作成されている。

## 2. XSS（クロスサイトスクリプティング）対策 [OWASP A07:2021 - Identification and Authentication Failures]

### **2.1 `innerHTML` の使用禁止**

- `innerHTML` を使用すると、悪意のあるスクリプトが埋め込まれるリスクがある。
- 代替として `textContent` や `innerText` を使用する。

#### **❌ NG例（XSSの危険あり）**

```javascript
const userInput = "<script>alert('XSS');</script>";
document.getElementById("output").innerHTML = userInput;
```

#### **✅ OK例（安全な方法）**

```javascript
const userInput = "<script>alert('XSS');</script>";
document.getElementById("output").textContent = userInput;
```

### **2.2 Content Security Policy (CSP) の適用**

- `script-src` に `unsafe-inline` を含めない。
- 可能な限り `nonce` や `hash` を使用し、外部スクリプトの制限を行う。

#### **CSP の例（推奨設定）**

```html
<meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self' 'nonce-random123';">
```

## 3. セキュアな API 通信 [OWASP A04:2021 - Insecure Design]

### **3.1 HTTPS の強制**

- API リクエストには `https://` を使用し、`http://` は許可しない。
- `fetch` や `axios` を使用する際は、常に `https://` で通信する。

### **3.2 CORS（Cross-Origin Resource Sharing）の適切な設定**

- フロントエンドからの API リクエストは、適切な `CORS` 設定を行う。
- `Access-Control-Allow-Origin: *` を避け、特定のオリジンのみ許可する。

#### **✅ OK例（許可するオリジンを限定）**

```http
Access-Control-Allow-Origin: https://example.com
```

## 4. クライアント側の認証情報管理 [OWASP A07:2021 - Identification and Authentication Failures]

### **4.1 JWT・OAuthトークンの保存方法**

- `localStorage` には認証トークンを保存しない（XSS攻撃で盗まれるリスクがある）。
- `HTTP-Only Cookie` を使用して保存する。

#### **❌ NG例（`localStorage` に保存）**

```javascript
localStorage.setItem("token", jwtToken);
```

#### **✅ OK例（`HTTP-Only Cookie` を使用）**

```javascript
document.cookie = "token=jwtToken; Secure; HttpOnly; SameSite=Strict";
```

### **4.2 セッションの適切な管理**

- セッション ID は毎回ログイン時に更新する。
- `SameSite=Strict` を推奨し、クロスサイト攻撃を防ぐ。

## 5. エラーハンドリングとロギング [OWASP A09:2021 - Security Logging and Monitoring Failures]

### **5.1 エラーメッセージの適切な制御**

- ユーザー向けのエラーメッセージにシステム内部情報を含めない。
- 詳細なエラー内容はサーバーログに記録し、ユーザーには一般的なメッセージを返す。

#### **❌ NG例（詳細なエラー情報を公開）**

```javascript
fetch("https://api.example.com/data")
  .then(response => response.json())
  .catch(error => alert(error.message)); // エラー詳細がユーザーに表示される
```

#### **✅ OK例（エラーメッセージを適切に処理）**

```javascript
fetch("https://api.example.com/data")
  .then(response => response.json())
  .catch(() => alert("データの取得に失敗しました。サポートにお問い合わせください。"));
```

## 6. 依存ライブラリの管理 [OWASP A06:2021 - Vulnerable and Outdated Components]

### **6.1 依存関係の定期的なスキャン**

- `npm audit` または `yarn audit` を定期的に実施する。
- 既知の脆弱性があるライブラリは、可能な限り最新バージョンに更新する。

#### **✅ OK例（脆弱性スキャンの実行）**

```sh
npm audit --json
```

### **6.2 信頼できないサードパーティスクリプトの使用制限**

- 外部スクリプト（CDNなど）は、公式のものを使用し、ハッシュを指定する。

#### **✅ OK例（SRI - Subresource Integrity の適用）**

```html
<script src="https://cdn.example.com/library.js" integrity="sha384-xxxx" crossorigin="anonymous"></script>
```

## 7. 参考資料

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [Google Web Fundamentals - Security](https://web.dev/security/)
- [Mozilla Developer Network (MDN) - Web Security](https://developer.mozilla.org/en-US/docs/Web/Security)
