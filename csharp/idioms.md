## イディオム

### if の中括弧の省略はしない

`if` の `{ }` (中括弧) は省略できます。
`return` のみだったり、同じような処理が並ぶ場合は、省略したくなります。
しかし省略することで、括弧によらず、インデントのみで区別される行ができてしまいます。
バグの元になりやすいので中括弧の省略はおすすめできません。

### 否定形の名前は避ける

メソッド名やプロパティ名等は肯定形で記述します。
条件式で否定が続くと読みにくくなります。
二重否定を避けるという理由もあります。

ただし否定形を用いることで文脈がわかりやすくなる場合は、許容します。

```cs
// NG: 二重否定
if (!isNotFound)

// OK: 肯定形
if (x.IsNotNull())
if (x.Exists())
if (x.Contains())
```

### マジックナンバーは使わない

冗長なようでもマジックナンバーの代わりに正しい名前を持つ変数を用意します。
また、その値を設定した理由をコメントに残しておくこともコードの理解を助けます。
ただし、文脈から意味が明確な場合は例外とします。

```cs
public static class Constants
{
    public static readonly int DaysInWeek = 7;
    public static readonly int HoursInDay = 24;
}
```

### 変数は一度だけ設定する

変数の値が途中で変更されるとソースが読みにくくなりますので、なるべく値は一度だけ設定するようにします。
フィールドの `readonly` 、ローカル変数の `const` は積極的に使います。

### ガード節は積極的に使う

メソッドの早い段階での `return` は積極的に行います。
ネストを減らすことができます。
最後にただひとつだけ `return` をするという方針は無意味です。
必ず実行したい処理がある場合には `try-finally` の適用を検討します。

### プロパティはステートレスにする

プロパティを設定する順番によって差があってはなりません。
例えば、`DataSource`と `DataMember` のプロパティは、
どちらを先に設定しても同じ結果になります。

設定順が重要な場合はメソッドを実装します。

### i++ と ++i

どちらを使っても問題ありません。
速度に与える影響は微々たるものですし、コンパイラによっても変わります。
ただし、式の評価順が結果に影響を与える場合は注意が必要です。
`i++`を式に埋め込むより、別の行にできないか検討します。

### for の比較演算子

`for` に用いる比較演算子は `<` を使うことが多いです。
これは配列がゼロベースのインデックスで用いられるためです。

```cs
for (var i = 0; i < length; i++)
```

### 比較演算子の向き

比較演算子は、左を主とするのが一般的です。
範囲を扱う場合は、比較演算子の左を小、右を大として、`<, <=`のみ使用する方がわかりやすくなります。

```cs
// 比較演算子の左を主とした場合
if ((a >= 90 && a <= 180) ||
    (a >= 270 && a <= 360))

// 比較演算子の向きを揃えた場合
if ((90 <= a && a <= 180) ||
    (270 <= a && a <= 360))
```

### null ではなく空の配列を返す

メソッドでリストや配列を返すときは、null ではなく空のインスタンスを返すようにします。
null チェックの煩雑さがなくなり、処理が簡潔になります。

### ループより LINQ を使う

LINQ を使うとループより意味を明確に記述できます。
クエリ構文は内部的にメソッド構文に変換して実行されます。
LINQ のフル機能にアクセスするためにはメソッド構文を使用する必要があります。

```cs
// 通常のループ
var evenMax = 0;
foreach (var d in decimals)
{
    if (d % 2 == 0)
    {
        if (d > evenMax)
        {
            evenMax = d;
        }
    }
}

// LINQ (メソッド構文 + ラムダ式)
var oddMin = decimals
    .Where(x => (x % 2) == 1)
    .Min();
```

### キャストは as および is を使う

C# 7.0 以降は `is` 式のパターンマッチングを使用できます。

```cs
// 推奨する記法 (C# 7.0)
if (obj?.Value is int i)
{
}

// Null 条件演算子 (C# 6.0)
if (obj?.Value is int)
{
    int i = (int)obj.Value;
}

// 非推奨: 古い記法 (C# 1.0)
if (obj != null &&
    obj.Value is int)
{
    int i = (int)obj.Value;
}

// 非推奨: as 演算子
var instance = obj as IDisposable;
if (instance != null)
{
    instance.Dispose();
}
```

### 変わった書き方は避ける

広く使われている書き方の方が読みやすいです。
コードは書きやすさより読みやすさを重視します。

```cs
// 特異な記述
// while ((len--) > 0)と等価
var len = 100;
while (len --> 0)
{
    // 99 .. 0 のループ
}

// 一般的な記述
for (var i = 99; i >= 0; i--)
{
    // 99 .. 0 のループ
}
```

### this. および ClassName. の推奨
`this.`は現在のインスタンスを参照します。
フィールドやメソッドを呼び出す場合、`this.`をつけることによって明示できます。
メソッド内で `this.` がついていたらローカル変数ではなくフィールドだと判別できます。

`base.`は `override` などで隠匿されたメンバへ明示的にアクセスする場合にのみ使います。

`static`なメンバにアクセスする場合には `ClassName.` (クラス名)を付けます。
冗長な場合は省略されることもありますが、`static class`でない場合は明示した方がよいでしょう。

ReSharper では省略が推奨されていますが、明示した方が間違いを減らせます。

<div class="break" />

### var の使いどころ
厳密な型が重要でない場合には `var` を積極的に使います。
変数名から `var` の型が推測できない場合は名前を修正してください。

`var n = 10m;`は `Decimal n = 10m;` と明示した方が間違いを減らせます。

ReSharper では `var` の使用を推奨されていますが、適用箇所は考えた方がよいでしょう。

### const と readonly の使い分け
`const`はコンパイル時定数ですが、`readonly`は実行時定数(変数)です。
`const`を使用するとデバッグ時のエディットコンティニュで変更できません。
アセンブリ内への定数の埋込が発生するので再コンパイルが必要になります。

属性(`attribute`)や列挙型(`enum`)の定義など、コンパイル時に定数が必要な箇所のみ `const` を使用します。

ReSharper では `const` の使用が推奨されていますが、代わりに `readonly` を使ってください。

### 解放が必要なオブジェクトには using を使う
`IDisposable`インターフェースを実装し、`Dispose()`メソッドが実装されているオブジェクトには using ステートメントを使います。
ファイル、画像、メモリ、ネットワーク、リソースを扱うオブジェクトは解放が必要となります。

```cs
try
{
    string path = "test.txt";
    using (var stream = new FileStream(path, FileMode.Read))
    using (var reader = new StreamReader(stream))
    {
        // 処理
    }
}
```

### Dispose() 後は null を設定する
オブジェクトは `Dispose()` 後すぐにメモリから開放されるわけではありません。
`null`を設定することにより危険な参照を避けることができます。
実行環境によっては `null` を設定しないとガベージコレクタで解放されない場合があります。

### catch 時の throw の使い分け
`throw ex;`でリスローとするとスタックトレースが上書きされます。
その場で処理するべきか、呼び出し元へ伝えるべきかで使い分けます。

```cs
catch (Exception1)
{
    // 原因を呼び出し元に伝える場合
    db.Rollback();
    throw;
}
catch (Exception2 ex)
{
    // 原因の詳細がそれ以上必要ない場合
    Debug.WriteLine(ex.ToString());
    throw ex;
}
```

<div class="break" />

### 引数でコレクションを受けるときはできるだけ抽象度の高いものを使う
メソッドの引数に `IEnumerable<T>` を指定することによって、引数の内容が変化しないことを明示できます。
`IEnumerable<T>`は"列挙可能"なことを示しますが、`IList<T>`は"追加・削除"が可能です。
.NET 4.5 以降は `IReadOnlyList<T>` もありますが、`IEnumerable<T>`の方が最初から順番にアクセスしていく意味が強いです。

```cs
public void DoAnything(IEnumerable<T> list)
```

### Array, ArrayList, List<T> の使い分け

* `System.Array` 
すべての配列の基本クラスです。`[]`で宣言します。 
固定長なのでパフォーマンスを考慮する場合や、画像やデータを保持する場合などに使います。 

* `System.Collections.ArrayList` 
サイズが動的に変動する配列です。 
型指定できないので `ArrayList` より `List<T>` を使います。

* `System.Collections.Generics.List<T>` 
型指定された `ArrayList` です。

```cs
// Array
byte[] bytes1 = new byte[2] { 0x00, 0xFF };
byte[] bytes2 = new byte[] { 0x00, 0xFF };
byte[] bytes3 = { 0x00, 0xFF };
var array = new[] { 0x00, 0xFF };

// List<T>
List<string> words = new List<string>();
words.Add("Hello");
words.Add("World");
words.Insert(1, ", ");
string[] array = words.ToArray();
```

### Hashtable, Dictionary<TKey, TValue> の使い分け

* `System.Collections.Hashtable` 
キー/値のペアの配列です。連想配列やハッシュ(ハッシュテーブル)などとも言います。 
型指定できない `Hashtable` より `Dictionary<TKey, TValue>` を使います。

* `System.Collections.Generic.Dictionary<TKey, TValue>` 
型指定された `Hashtable` です。

```cs
// Dictionary<TKey, TValue>
var dic = new Dictionary<int, string>();
int key = 0;
dic.Add(key, "value");
if (dic.ContainsKey(key))
{
    string data = dic[key];
}
```

### 一定時間スリープさせる方法
スリープさせるにはいくつかの方法があります。
`Wait()`は `await` と組み合わせるとデッドロックが発生するので注意が必要です。

```cs
int ms = 1000;

// 古い書き方
// スレッドごと停止します。
Thread.Sleep(ms);

// .NET4以降
Task.Delay(ms).Wait();

// asyncメソッド内であればawaitを使います。
await Task.Delay(ms);
```

<div class="break" />

### アクセス修飾子について

アクセス修飾子は省略できますが、明示した方が間違いを減らせます。
`protected`と `internal` は依存関係を複雑にするため、あえて使用しない場合もあります。

アクセシビリティレベルの制限は下記の通りです。
- `public`: 制限なし
- `private`: 自身(クラス内) のみ許可する
- `internal`: 同一プロジェクト内(同一アセンブリ)に許可する
- `protected internal`: 同一プロジェクト内 または 派生クラスに許可する
- `protected`: 派生クラスに許可する
- `private protected`: 同一プロジェクト内の派生クラスに許可する

アクセス修飾子を省略した場合のアクセシビリティレベルは、下記の通りです。
- トップレベルの型: `internal` (同一アセンブリ内のみ)
- `class, struct` のメンバー: `private`(クラス内のみ)
- `interface` のメンバー: `public`
- `enum` のメンバー: `public`

### 修飾子の順番

修飾子は、下記の順番が推奨されています。

```cs
public protected internal private new abstract virtual override sealed static readonly extern unsafe volatile async
```

ReSharper で推奨されています。

### プリミティブ型(エイリアス)と CLS 型の使い分け

型を宣言する場合はプリミティブ型 (`object, int, string` など) を使います。
静的メンバーを参照する場合は CLS 型 (`Object, Int32, String`など) を使います。

MSDN ではエイリアスを活用するよう推奨されますが、クラスの意味を考えると CLS 型を使ったほうがスマートです。

```cs
if (String.IsNullOrEmpty(str))

if (Decimal.TryParse(txt, out decimal n))
```

### unsigned 型はなるべく使わない

`uint`すなわち `System.UInt32` は CLS に準拠していません。
`uint`ではなく、`int`を使用します。
`int`を使用すると他のライブラリと対話しやすくなります。

### interface と abstract class の使い分け
`interface`にメンバを追加すると、既存のコードを破壊します。
将来にわたって変更がない場合や、多重継承が必要な場合にのみ `interface` を使用します。
`interface`に共通のメソッドが必要な場合は拡張メソッドの使用を検討します。
それ以外は `abstract class` を使用しましょう。

### 拡張メソッドについて
拡張メソッドは `enum, interface` などメソッドが追加できないものに使います。
それ以外はインスタンスメソッドや静的メソッドで解決できないか検討します。

```cs
public static class FooExtensions
{
public static void Method(this IFoo foo) { }
}
```

`System.Linq`には `IEnumerable` に対しての拡張メソッドが用意されています。

### コードの依存関係
コードを共有化(ライブラリ化)することで、再利用性を高めることは重要です。
しかし、共有化するということは、ライブラリへの依存度を高めます。
ライブラリの変更が広範囲にわたる場合は、保守のコストが増大します。
共有化する場合は、ライブラリが肥大化しないよう、最小限の機能となるよう検討します。

### ユーティリティクラスの是非
重複を避けるため共通の処理をユーティリティクラスとし、コードを再利用するのは便利です。
しかしオブジェクト指向的ではなく、手続き型的であり、クラスの再利用性に問題があります。
多くのクラスがユーティリティクラスを呼び出し、依存してしまうことが予想されます。
ユーティリティクラスを作りたくなったら、本当に必要か検討します。

例えばアプリケーション全体で、デバッグ用のログを出力したくなったとします。
汎用ユーティリティクラスの中の一部にログ出力がある場合は、専用の `Log` クラスまたは `Logging` パッケージにできないか検討します。

例えば `String.IsNullOrEmpty()` というメソッドがあり、他のクラスでも似た機能を利用したくなったとします。
汎用ユーティリティクラスの中に `IsNullOrZero()` があるのは、オブジェクト指向的ではありません。
可能であれば、対象のクラスを継承し、必要なメソッドを追加しましょう。
不可能であれば、専用ユーティリティクラスにできないか検討します。
多数に追加する必要があるならば、インターフェイスと拡張メソッドの組み合わせを検討します。

## コメント

正しい名前と構造を持ったコードは、コメントがなくても読みやすいですが、コードの要約 (summary) を読む方が簡単です。
必要な箇所に必要なコメントを記述することはコードの品質を高めます。
コードを復唱するコメントはいりません。
コードを抽象レベルで説明するコメントを書いてください。
TODO(実装予定), UNDONE(実装中), HACK(改良予定)などトークンも併せて使うとよりわかりやすくなります。

### コメント タグ

C# ドキュメンテーション コメント形式で記述すればインテリセンス (IntelliSense) へ反映されます。
Sandcastle などのツールを利用すれば API ドキュメントを作成することも可能です。
VC#で `///` と入力すれば自動的に補完されます。

* summary 
要約です。何をするコードか 1 行で簡潔に記述してください。インテリセンスで表示されます。
* remarks 
解説です。要約を補足します。
* param 
コンストラクタやメソッドのパラメータの説明を記述します。name プロパティによりパラメータ名を指定できます。
* returns 
メソッドの戻り値の説明を記述します。結果が `bool` などで説明の必要がなければ省略してもかまいません。

```cs
/// <summary>
/// MyMethodの要約です。1行で記述します。
/// </summary>
/// <remarks>
/// MyMethodの補足です。
/// 細かい内容はこちらに記述します。
/// </remarks>
/// <param name="x">xの説明</param>
/// <returns>戻り値の説明</returns>
public bool MyMethod(int x)
```

* para 
summary や remarks 内で段落を分けたい場合に使用します。

```cs
/// <summary>
/// <para>MyMethod の要約です。</para>
/// <para>para を使えば複数行も記述できます。</para>
/// </summary>
/// <remarks>
/// <para>MyMethod の補足です。</para>
/// <para>改行や段落を表現したいときは para を使います。</para>
/// </remarks>
public bool MyMethod(int x)
```

* value 
プロパティが表す値の説明を記述します。

```cs
/// <summary>
/// Name プロパティです。
/// </summary>
/// <value>Name プロパティの値の説明</value>
public bool Name { get; }
```

<div class="break" />

* exception 
例外の説明を記述します。cref プロパティによりスローできる例外を指定できます。 
メソッド、プロパティ、イベントに対応します。

```cs
/// <exception cref="System.ArgumentException">例外の説明です。</exception>
/// <exception cref="System.Exception">例外の説明です。</exception>
public void ThrowException(params object[] args)
```

* see
文中にリンクを追加できます。cref プロパティによりクラスまたはメンバを指定できます。

```cs
/// <summary>
/// 内部で<see cref="MyClass.MyMethod(System.String)"/>を呼び出しています。
/// </summary>
```

* seealso
参照項目として追加できます。cref プロパティでメンバを指定できます。 
href プロパティは Sandcastle で API ドキュメントを作成する際に有効です。

```cs
/// <seealso cref="MyClass"/>
/// <seealso cref="MyClass2.MyMethod(int)"/>
/// <seealso href="http://www.google.com/"/>
```

* `example`
使用例を記述します。コード部分は c または code タグを使用します。
* `c`
1 行のみのコードを記述します。説明内でコードが必要な場合に使用します。
* `code`
複数行のコードを記述します。

```cs
/// <example>
/// 使い方:
/// <c>int x = await ReadAsync(...);</c>
/// </example>
```

### インテリセンスの表示例

VS では下記のように表示されます。

```cs
/// <summary>
/// ログに Debug メッセージを出力します。
/// </summary>
/// <param name="format">出力メッセージ (複合書式指定可)</param>
/// <param name="args">パラメータ (省略可)</param>
public static void d(string format, params object[] args)
```

<!-- ![インテリセンスの表示例](./doc_devel_csCodingGuidelines_CommentTag.png "インテリセンスの表示例") -->
![インテリセンスの表示例](https://qiita-image-store.s3.amazonaws.com/0/91573/1f821e2c-20b7-cbff-6190-225a9b80f990.png "インテリセンスの表示例")
