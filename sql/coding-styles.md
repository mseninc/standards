## コーディングスタイル

<!-- TOC -->

- [コーディングスタイル](#コーディングスタイル)
  - [基本ルール](#基本ルール)
    - [カラムや値の区切りカンマは行頭に配置する](#カラムや値の区切りカンマは行頭に配置する)
    - [SQL の特定の予約語の後は改行する](#sql-の特定の予約語の後は改行する)
    - [1 カラム 1 行にする](#1-カラム-1-行にする)
    - [条件の AND / OR は先頭に置く](#条件の-and--or-は先頭に置く)
    - [CASE 文は各予約語の前で字下げして改行する](#case-文は各予約語の前で字下げして改行する)
    - [JOIN 句は字下げして、ON は更に字下げする](#join-句は字下げしてon-は更に字下げする)
    - [セミコロンは最終行に置く](#セミコロンは最終行に置く)
  - [サブクエリ](#サブクエリ)
    - [原則としてサブクエリは WITH を使う](#原則としてサブクエリは-with-を使う)
    - [サブクエリは字下げする](#サブクエリは字下げする)

<!-- /TOC -->

### 基本ルール

- インデントにはタブ文字は使わず、半角スペース 2 つとします。
- SQL の予約語 (SELECT, FROM, WHERE, ...) および標準関数 (SUM, COUNT, ...) は大文字とします。
- SQL をファイルで保存する場合は拡張子を `.sql` とし、 文字コードは UTF-8 (BOM なし) とします。
- 列や値を区切るカンマは行の先頭に記載します。
- キーワードやリテラルの間はスペースで区切ります。演算子の前後 (`=`, `>`, ...)、カンマ (`,`) の後、文字列リテラルの前後など、省略可能な場合でも省略してはいけません。

```sql
-- Good
SELECT
  COUNT(*) AS sushi_post_count
FROM microposts
WHERE content LIKE '%:sushi:%'
  AND posted_at > '2020-01-01'
;

-- NG
select
    count(*) as sushi_post_count
from microposts
where content like '%:sushi:%'
    AND posted_at>'2020-01-01'
;
```

#### カラムや値の区切りカンマは行頭に配置する

カラムや値を区切るカンマは次行の行頭にスペース 1 つを空けて配置します。

```sql
-- Good
SELECT
  user_id
, hashed_password
, created_at
FROM users
WHERE user_id IN (
    100
  , 2000
  , 300
  , 4000
  )
;

-- NG
SELECT
  user_id,
  hashed_password,
  created_at
FROM users
WHERE user_id IN (
    100,
    2000,
    4000
    ,300
  )
;
```

#### SQL の特定の予約語の後は改行する

SQL の構文構造を決める予約語の後は改行します。ただし、保守性および可読性を向上させるため FROM, WHERE, UPDATE, INSERT INTO は除きます。

```sql
SELECT
SET
GROUP BY
ORDER BY
```

逆にこれ以外の予約語については、一行が長くなりすぎたり CASE 文などでない限り、改行を省略します。

```sql
-- Good
SELECT
  DATE(created_at) AS created_date
, COUNT(*) AS post_count
FROM posts
WHERE created_at BETWEEN '2015-12-01' AND '2015-12-10'
GROUP BY
  created_date
;

UPDATE posts
SET
  content = 'hoge'
WHERE id = 1
;

INSERT INTO posts (
  id
, content
, created_at
) VALUES (
  5
, 'Hello, world'
, '2020-01-01T00:00:00Z'
)
;

-- NG
SELECT DATE(created_at) AS created_date
, COUNT(*)
  AS post_count
FROM posts
WHERE created_at BETWEEN
 '2015-12-01' AND
 '2015-12-10'
GROUP BY created_date
;

UPDATE
  posts
SET content = 'hoge'
WHERE id = 1
;

INSERT INTO
posts (
  id
, content
, created_at
) VALUES (
  5
, 'Hello, world'
, '2020-01-01T00:00:00Z'
)
;
```

ただし、 `SELECT DISTINCT` については続けて記載します。

```sql
SELECT DISTINCT
  gender
FROM users
;
```

#### 1 カラム 1 行にする

SELECT、WHERE、GROUP BY、ORDER BY で指定するカラムや条件式は、1 行に 1 つまでとし、1 行に複数を記述しないようにします。

```sql
-- Good
SELECT
  DATE(created_at) AS created_date
, user_id
, COUNT(*) AS post_count
FROM posts
WHERE created_at BETWEEN '2015-12-01' AND '2015-12-10'
  AND post_type = 'article'
GROUP BY
  created_date
, user_id
ORDER BY
  created_date
, post_count
;

-- NG
SELECT
  DATE(created_at) AS created_date, user_id, COUNT(*) AS post_count
FROM posts
WHERE created_at BETWEEN '2015-12-01' AND '2015-12-10' AND post_type = 'article'
GROUP BY
  created_date, user_id
ORDER BY
  created_date, post_count
;
```

#### 条件の AND / OR は先頭に置く

AND / OR は行の先頭におき、ブロックよりインデントを一段下げて記述します。

```sql
-- Good
SELECT
  COUNT(*) AS user_count
FROM user
WHERE created_at >= '2015-12-01'
  AND status IN ('signup', 'available')
  AND admin IS NULL
;

-- NG
SELECT
  COUNT(*) AS user_count
FROM user
WHERE created_at >= '2015-12-01' AND
  status IN ('signup', 'available') AND
  admin IS NULL
;
```

#### CASE 文は各予約語の前で字下げして改行する

CASE と END をカッコ () に見立てて、それぞれ字下げを行います。
どこからどこまでが CASE 文なのか、条件式はどこまでかをわかりやすくするために、改行・字下げをします。

```sql
CASE
  WHEN hoge
    THEN 'hoge'
  WHEN piyo
    THEN 'piyo'
  ELSE 'other'
END
```

```sql
-- Good
SELECT
  CASE
    WHEN DATE(created_at) = DATE(NOW())
      THEN 'today'
    WHEN DATE(created_at) = DATE(NOW()) - INTERVAL 1 day
      THEN 'yesterday'
    ELSE 'other_days'
  END AS created_date_category
, COUNT(*) AS post_count
FROM microposts
;

-- NG
SELECT
  CASE
  WHEN DATE(created_at) = DATE(NOW()) THEN 'today'
  WHEN DATE(created_at) = DATE(NOW()) - INTERVAL 1 day THEN 'yesterday'
  ELSE 'other_days'
  END AS created_date_category
, COUNT(*) AS post_count
FROM microposts
;
```

単純 CASE の場合は CASE の直後に式を配置します。

```sql
SELECT
  CASE post_type
    WHEN 'article'
      THEN '投稿'
    WHEN 'news'
      THEN 'ニュース'
    ELSE 'その他'
  END AS post_type_name
FROM posts
;
```

#### JOIN 句は字下げして、ON は更に字下げする

JOIN 句は字下げして、ON とそれに続く条件式は更に字下げします。

```sql
-- Good
SELECT
  COUNT(*) AS post_count
FROM microposts AS m
  INNER JOIN users AS u
    ON m.user_id = u.id
    AND u.created_at > '2020-01-01'
  LEFT JOIN countries AS c
    ON c.id = u.country_id
WHERE u.status IN ('signup', 'available')
;

-- NG
SELECT
  COUNT(*) AS post_count
FROM microposts AS m
INNER JOIN users AS u
ON m.user_id = u.id
AND u.created_at > '2020-01-01'
LEFT JOIN countries AS c
ON c.id = u.country_id
WHERE u.status IN ('signup', 'available')
;
```

#### セミコロンは最終行に置く

セミコロンを SQL 文の最終行の行頭に配置します。
どこで SQL が終わっているかがわかりやすくなります。

```sql
-- Good
SELECT
  COUNT(*) AS sushi_beer_post_count
FROM microposts
WHERE DATE(created_at) = DATE(UTC_TIMESTAMP())
  AND content REGEXP(':(sushi|beer):')
;

-- NG
SELECT
  COUNT(*) AS sushi_beer_post_count
FROM microposts
WHERE DATE(created_at) = DATE(UTC_TIMESTAMP())
  AND content REGEXP(':(sushi|beer):');
```

### サブクエリ

#### 原則としてサブクエリは WITH を使う

WITH 句が実装されている DBMS では WITH 句を用いてサブクエリを抽出します。
WITH はカンマで区切って、複数のクエリを書くことができます。

```sql
WITH user_post_count AS (
  SELECT
    user_id
  , COUNT(*) post_count
  FROM
 microposts
  GROUP BY
    user_id
)

SELECT
  post_count
, COUNT(*) AS freq
FROM user_post_count
GROUP BY
  post_count
;
```

#### サブクエリは字下げする

サブクエリはインデントを一段下げ、範囲を明確にします。
ただし、できるだけ WITH を使って書くことを推奨します。

```sql
-- Good
SELECT
  post_count
, COUNT(*) AS freq
FROM (
    SELECT
      user_id
    , COUNT(*) post_count
    FROM
   microposts
    GROUP BY
      user_id
  )
GROUP BY
  post_count
;

-- NG
SELECT
  post_count
, COUNT(*) AS freq
FROM
(
SELECT
  user_id
, COUNT(*) post_count
FROM microposts
GROUP BY
  user_id
)
GROUP BY
  post_count
;

```

EXISTS 句も同様に括弧内をインデントします。カッコは EXISTS と同階層とします。

```sql
-- Good
SELECT
  u.name
FROM users AS u
WHERE 1 = 1
  AND EXISTS
  (
    SELECT
      1
    FROM
   microposts
    WHERE user_id = u.id
  )
;

-- Good
SELECT
  u.name
FROM users AS u
WHERE EXISTS
  (
    SELECT
      1
    FROM
   microposts
    WHERE user_id = u.id
  )
;
```
