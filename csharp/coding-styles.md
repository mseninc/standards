
## コーディング規則

### レイアウト規則

* 1 つの行には 1 つの宣言のみ記述します。
* 1 つの行には 1 つのステートメントのみを記述します。
* メソッドとプロパティの各定義の間には 1 つ以上の空白行を追加します。
* タブ文字は使わず、スペース 4 つとします。
* 継続行には 1 タブ分 (スペース 4 つ) のインデントを行います。
* 式 (expression)に句 (clause) を含めるときは括弧でくくります。

### コメント規則

* コード行の末尾ではなく別の行に記述します。
* 文章となるように記述します。(先頭大文字、ピリオドで終わる)
* コメント記号 (`//`) と文字の間には空白をひとつ挿入します。

```cs
var a = 0; // ダメなコメントのパターンです。

// コメントは別の行に記述します。
var a = 0;
```

例外としてメソッドチェーンの各行にコメントを記述する場合のみ行の末尾も許容します。
ただしコメントが長くなる場合は前行に挿入してください。

```cs
var a = this.CreateList()
    .Where(x => x.Length > 3) // 長さが 3 桁以上なら
    .Select(x => x.Length * 3) // 各長さを 3 倍した数値にして
    .ToArray(); // 配列に変換する
```

### 長い名前

1 行に収まらない場合は `.` (ピリオド) の後で改行を行うことができます。

```cs
var currentPerformanceCounterCategory = new System.
    Diagnostics.PerformanceCounterCategory();
```

### 多い引数

引数が多く、1 行が長くなる場合は `(` (括弧) や `,` (カンマ) の後で改行できます。
引数の数が多くなる場合は Builder パターンの適用も検討しましょう。
メソッドチェーンを記述する場合はピリオドを前置とします。

```cs
var ipAddr = String.Format(
    "{0}.{1}.{2}.{3}",
    0x7F, 0x00, 0x00, 0x01);

var connStr = new StringBuilder()
    .AppendFormat("Server={0};", "localhost")
    .AppendFormat("Database={0};", "master")
    .AppendFormat("User Id={0};", "sa")
    .AppendFormat("Password={0};", "sa")
    .ToString();
```

### 多い演算子

演算子が多く、1 行が長くなる場合は演算子の前後で改行できます。

```cs
bool isEmpty = ((data == null)
    || (data == DBNull.Value)
    || (String.IsNullOrEmpty(data.ToString())));

if ((data == null) ||
    (data == DBNull.Value) ||
    (String.IsNullOrEmpty(data.ToString())))
{
    // 処理
}
```

### 三項演算子

三項演算子で各項が長くなる場合は、 `?`, `:` の前で改行します。

```cs
// 項が短い場合
var n = isVisible ? 1 : 0;

// 項が長い場合
var n = list.Any(x => x.Number > 0)
    ? list.Select(x => x.Number).Sum()
    : 0;
```

### this

自クラスのメンバーにアクセスする際の `this` は省略します。

- NG: `this.Member = "x";`
- OK: `Member = "x";`

拡張メソッドを呼び出す場合は、 `this` が必要なため、許容します。

- OK: `this.IAmExtensionMethod();`

### 自動プロパティ

自動プロパティは 1 行で記述します。

```cs
// prop -> tab -> tabで補完できます。
public int MyProperty { get; set; } = 0;
```

### 空のコンストラクタ

空のコンストラクタは 1 行で記述します。

```cs
// ctor -> tab -> tabで補完できます。
public int MyConstructor { }
```

### イベント処理

イベントにはイベントハンドラーを関連付けます。
処理が十分に短い場合や再利用が不要な場合はラムダ式で記述します。

イベントの数が多いと見通しが悪くなりますし、Tab 補完や `partial` (コードビハインド) の記述に合いません。

```cs
public SampleForm()
{
    // イベントハンドラの設定
    // Event名 += -> tab -> tabで補完できます。
    Click += SampleForm_OnClick;
    // イベントハンドラの削除
    Click -= SampleForm_OnClick;
    
    // 推奨する記法 ラムダ式を使う場合 (C#3.0)
    Click += (s, e) =>
    {
        // TODO: 処理
    };
}

// イベントハンドラー
private void SampleForm_OnClick(object sender, EventArgs e)
{
    // TODO: クリックされたときの処理を実装します。
}
```
