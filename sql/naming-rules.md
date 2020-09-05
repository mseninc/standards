## 命名規則

<!-- TOC -->

- [命名規則](#命名規則)
  - [エイリアス](#エイリアス)

<!-- /TOC -->

### エイリアス

JOIN するテーブルやサブクエリには必ず AS によってエイリアスを付与します。

- テーブル名の単語から頭文字をとる
- テーブル名をそのまま利用する (ただし長いテーブル名で可読性が下がる場合は一部を省略するなどして可読性を重視する)

- エイリアス名の大文字小文字はそのドメイン内でテーブル名と合わせる (テーブル名が大文字のときはエイリアス名も大文字)
- 名前が重複してしまう場合は、それぞれが判別できるように数字もしくは追加の説明をつける
- エイリアスを識別しやすくするため AS は省略しない。

```sql
-- Good
users AS u
D_USER AS U
D_USER AS USER
user_groups AS ug
user_authentication_tokens AS auth_token

-- NG
users AS A
D_USER AS u
D_USER AS user
user_groups AS B
user_authentication_tokens AS user_authentication_token
```

- 例. 同じテーブルを結合する場合でそれぞれに別の意味がある場合

```sql
FROM
  users AS user
  INNER JOIN users AS user_active
```

- 例. 同じテーブルを結合する場合で結合順に意味がある場合

```sql
FROM
  users AS u1
  LEFT JOIN users AS u2
```
