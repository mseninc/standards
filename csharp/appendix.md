## 付録

### 逐語的リテラル文字列

@を付けるとエスケープを省略でき、ヒアドキュメントとして扱えます。
Verbatim String Literal と呼ばれます。

```cs
// string path = "C:\\Users\\UserName\\Documents";
string path = @"C:\Users\UserName\Documents";

string sql = @"
SELECT
u.user_name
, o.order_date
FROM
users u
INNER JOIN orders o
ON o.user_id = u.user_id
WHERE
u.user_id = @user_id
AND o.order_id = @order_id'
;";
```

### 複合書式指定

書式を指定して文字列を作成できます。

書式は下記の通りです。
alignment は `-` (マイナス) で左寄せとなります。
`{index[,alignment][:format]}`

C#6 以降は**文字列補間 (Interpolated Strings) の書式**が推奨されます。

```cs
// var coord = String.Format("({0}, {1})", x, y);
string coord = $"({x}, {y})";
```

- [$ - 文字列補間 - C# リファレンス | Microsoft Learn](https://learn.microsoft.com/ja-jp/dotnet/csharp/language-reference/tokens/interpolated)

#### 数値の場合(標準)

- `Cn`: 通貨(n は小数桁)
- `Dn`: 10 進数(n は桁数)
- `En`: 指数(n は小数桁)
- `Fn`: 小数(n は小数桁)
- `Gn`: 数値(n は有効桁)
- `Nn`: 数値(n は小数桁)
- `Pn`: パーセンテージ(1 で 100%, n は小数桁)
- `R`: ラウンドトリップ(Single, Double, BigInteger のみ)
- `Xn`: 16 進数(FF, n は桁数)

※n は省略できます。
※大文字小文字の区別はありません。

```cs
float f = 0.1F;
int yen = 1000;
byte b = 0xFF;
// f=10.00%
string percent = $"f={f:P2}";
// yen=    \1,000
string currency = $"yen={yen,10:C}";
// b=0x00FF
string hex = $"b=0x{b,-5:X4}";
```

<div class="break" />

#### 数値の場合(カスタム)

- `0`: ゼロ プレースホルダー
- `#`: 桁プレースホルダー
- `%`: パーセンテージ
- `.`: 小数点
- `,`: 桁区切り記号
- `;`: セクション区切り記号(正,負,ゼロ)

```cs
string percent = $"{f:0.00%}";
string phone = $"{tel:(###)###-####}";
string section = $"{n:#;(#);Zero}";
```

#### 日付の場合

- `d`: 短い形式の日付(yyyy/MM/dd)
- `D`: 長い形式の日付(yyyy 年 M 月 d 日)
- `g`: 短い形式の日時(yyyy/MM/dd hh:mm)

※カルチャに依存します。

- `g, gg`: 年号(西暦)
- `y, yy, yyy, yyyy`: 年
- `M, MM`: 月
- `MMM, MMMM`: 月名(M, M 月)
- `d, dd`: 日にち
- `ddd, dddd`: 曜日(日, 日曜)
- `f, ff, fff, ...`: 秒以下の値
- `h, hh`: 時間(12 時間形式)
- `H, HH`: 時間(24 時間形式)
- `m, mm`: 分
- `s, ss`: 秒
- `tt`: AM/PM(午前/午後)
- `K`: タイムゾーン(+09:00)
- `:`: 時刻の区切り記号
- `/`: 日付の区切り記号

※TimeSpan 型の場合は、区切り記号が使えないのでエスケープする必要があります。

```cs
// string date = DateTime.Now.ToString("yyyy/MM/dd HH:mm:ss.fff");
// string date = String.Format("{0:yyyy/MM/dd HH:mm:ss.fff}", DateTime.Now);
string date = $"{DateTime.Now:yyyy/MM/dd HH:mm:ss.fff}";

// string date = TimeSpan.MaxValue.ToString(@"yyyy\/MM\/dd HH\:mm\:ss\.fff")
var date = $@"{TimeSpan.MaxValue:yyyy\/MM\/dd HH\:mm\:ss\.fff}";
```

#### 列挙型の場合

- `G, F`: 文字列
- `D`: 整数
- `X`: 16 進数 (0xFF)

※大文字小文字の区別はありません。
