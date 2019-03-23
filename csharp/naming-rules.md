## 命名規則

正しい名前付けを心掛けることにより、コードが洗練されます。
正しい名前を付けるには、正しいクラス構造が必要だからです。
意味のある、わかりやすい、正しい名前付けを心がけてください。

ただし、既存のコードを修正する場合は、そのスタイルに従ってください。
他のスタイルを混入させることは、品質の劣化となります。

### サンプルコード

```cs
namespace Site.Solution.Project
{
    public sealed class SingletonClass
    {
        #region singleton pattern
        
        private static SingletonClass Instance;

        private SingletonClass() { }

        public static SingletonClass GetInstance()
        {
            if (SingletonClass.Instance == null)
            {
                SingletonClass.Instance = new SingletonClass();
            }
            return SingletonClass.Instance;
        }
        
        #endregion

        public void DoSomething(params string[] args)
        {
            if (args == null)
            {
                return;
            }
            
            bool exists = args.Any(x => !string.IsNullOrWhiteSpace(x));
            int count = args.Count(x => !string.IsNullOrWhiteSpace(x));
            var strings = args.Where(x => !string.IsNullOrWhiteSpace(x));
            
            foreach (var s in strings)
            {
                Console.WriteLine(s);
            }
        }
    }

    public class FieldAndProperty
    {
        private int privateField;
        private readonly int readonlyPrivateField;

        // "const" is pascal case.
        // don't use "const". "readonly" instead.
        //private const float Zero = 0f;
        //public const float Pi = 3.1415927f;

        // "static" is pascal case.
        private static int PrivateStaticField;
        private static readonly int DaysInWeek = 7;
        public static readonly int DayOfWeekCount = 7;

        // don't use public fields. properties instead.
        //public int PublicField;
        //public readonly int ReadonlyPublicField;
        //public static int ClassField;

        // "property" is pascal case.
        public Color Color { get; set; }
    }
}
```

<div class="break" />

### 共通スタイルの説明
Pascal / Camel 形式を基本とします。
大文字 / 小文字の違いのみによる区別は避けてください。

- Pascal 形式: `PascalCaseName, IoException, XmlTransfer`
- Camel 形式: `camelCaseName, ioException, xmlTransfer`

以下の識別名は Camel 形式とし、それ以外は Pascal 形式とします。
- パラメータ
- private なフィールド
- ローカル変数

コメント以外は半角英数のみで記述し、英単語を使用してください。
日本語名が必要な場合は、コメントに含めます。ローマ字綴りは禁止とします。

スコープの短い局所変数では省略形を許可しますが、それ以外では単語は省略せずに記述してください。
例えば、表記揺れで `jpeg, jpg`, `message, msg`, `window, win, windows` などが混ざることは避けてください。

コレクションやリストの変数名は複数形とします。

- 単数形: `File file`
- 複数形: `List<File> files, byte[] bytes`

### 二文字の名前

ID, OK, IP, DB など二文字からなる言葉は短い名前といいます。
MSDN では二文字からなる短い頭字語はすべて大文字としています。
ID(Identifier)と OK(Okay)は頭字語ではなく、省略形となり区別が必要となります。
ここでは MSDN 形式を推奨しますが、規定とはせず、表記ゆれも許容します。

- Pascal 形式(MSDN): `IdManager, OkButton, IPAddress, DBTable, IOReader, XmlReader`
- Camel 形式: `idManager, okButton, ipAddress, dbTable, ioReader, xmlReader`

### 名前空間

Pascal 形式とします。
名前空間は一意でなければなりません。
社名、ソリューション名(製品名)、プロジェクト名(部品名)などを組み合わせてピリオドでつなぎます。
ドメイン名を利用してもかまいません。

#### 名前空間の例

- `<Site>.<Solution>.<Project>`
- `MSEN.<Product>.<Component>`

### ソリューション名、プロジェクト名

プロダクト名がある場合はそれを利用します。

ツールなどプロダクト名がない場合は、原則として `-er`, `-or` 形の単語を末尾にし、そのツールがなにをするかを明確にします。

- プロダクト名の例: `Knowrage`
- ツール名の例: `DbDumpManipulator`

テストプロジェクト名はテスト対象のアセンブリ名＋接尾辞 `Test` とします。

- `Hogehoge.Common` のテストプロジェクト → `Hogehoge.Common.Test`

### アセンブリ

実行ファイルはプロジェクト名 (`ProjectName.exe`) を用います。
DLL は名前空間およびプロジェクト名 (`Site.Solution.ProjectName.dll`) を用います。

- 実行ファイル: `ProjectName.exe`
- DLL ファイル: `Site.SolutionName.ProjectName.dll`

### リソース

Pascal 形式とします。

階層構造を明示し、使用箇所を明確にします。
共通で使うリソースなどもあるので、わかりやすいリソース名としましょう。

リソース: `Menu.File.Open.Icon, ArgumentNullExcception.Message`

### ファイル

クラス名と同じ名前にします。

基本的には 1 ファイルにつき 1 クラスですが、その他のファイルから参照していない依存度の高いクラスは、１つのファイルにまとめるか内部クラスにします。

<div class="break" />

### クラス

Pascal 形式とします。役割を表す名前をつけます。

処理中心のクラスには接尾辞として `-er`, `-or` をつけます。
派生クラスは、基本クラス (継承元) の名前を含めます。

- クラス: `String, Window, Task`
- 処理中心のクラス: `StringBuilder`
- `Exception` を継承したクラス: `ArgumentException, ArgumentNullException`
- `EventArgs` を継承したクラス: `KeyDownEventArgs`

拡張メソッドを定義するためのクラスは対象のクラスに接尾辞 `Extensions` を付加します。

- `string` の拡張メソッドを定義するクラス: `static class StringExtensions`

### プロパティ

Pascal 形式とし、名詞、形容詞を用います。

C# 3.0 以降は自動プロパティにより、get/set アクセサの中身を省略できるようになりました。
元になる型があれば、プロパティ名と型名は同じにしてください。

論理値を返すメソッドには `Is-, Can-, Has-` を使います。
特別な場合を除き `Is+<形容詞>` 、`Can+<動詞>`、`Has+<過去分詞>` とします。
エラーがあることを示すプロパティのみ `Has+<名詞>` を許可します。

- プロパティ: `Name, Closed, Size, Position, Length`
- 型と同じプロパティ: `public System.Drawing.Color Color { get; set; }`
- 論理値を返すプロパティ: `IsOpened, IsClosed, IsEnabled, IsVisible, HasShown, HasError`

以下に当てはまる場合はプロパティではなくメソッドを使用します。

* 処理に時間がかかる場合
* パラメータの変更なしに異なる結果が返る場合

### フィールド

`private` なフィールドは Camel 形式とし、名詞形を用います。
`public, internal` なフィールドは禁止します。

フィールドは、インスタンスフィールドやメンバ変数とも呼ばれます。

例外的にプロパティのバッキングフィールドの場合は、 接頭辞として `_` (アンダースコア) を付け、 Pascal 形式とします。これはコードスニペットの簡略化とプロパティ名の変更に伴う修正を容易にするためです。

論理値を格納する変数に限り、例外的に、`is-`, `can-`, `has-` を用いることができます。

静的フィールド (`static` フィールド, クラス変数) も同様の扱いとします。

```cs
// private field
private string testAccount;

// backing field
private bool _IsEnabled;
public bool IsEnabled { get => _IsEnabled; set => _IsEnabled = value; }

// static field
private bool hasInstance;
```

### コントロールのフィールド

Pascal 形式とし、名詞形を用います。原則としてコントロールを示す名称を末尾に付加します。

Windows Forms アプリケーションのコントロール名に限り、ハンガリアン形式で記述することを許可しますが、表記揺れで別の形式が混ざることは避けてください。

- コントロール名: `UserNameLabel, UserNameTextBox, PasswordLabel, PasswordMaskedTextBox, OkButton`
- ハンガリアン記法: `lblUserName, txtUserName, lblPassword, mtxPassword, btnOk`

イベントメソッドは `<コントロール名>+<イベント名>` とします。
イベントメソッドは Pascal 形式に修正が必要です。

- イベントメソッド: `BtnOk_Click, TxtUserName_Validating`
- 共通イベントメソッド: `Window_Loaded, TextBox_GotFocus, TextBox_KeyDown`


<div class="break" />

### メソッド (≒関数、サブルーチン、ファンクション)

Pascal 形式とし、動詞を用います。

インスタンスを生成するメソッドは `Create-` を使います。
論理値を返すメソッドには `Is-, Can-, Has-` を使います。特別な場合を除き `Is+<形容詞>` 、`Can+<動詞>`、`Has+<過去分詞>` とします。
型を変換するメソッドは例外的に `To-` を使います。

- インスタンスを生成するメソッド: `CreateInstance, NewInstance`
- 論理値を返すメソッド: `IsNull, IsNullable, IsEmpty, CanRead, HasClosed`
- 型を変換するメソッド: `ToString, ToInt32`

### 非同期メソッド

メソッドの命名規則に従います。同名で同期メソッドが存在する場合は、接尾辞として `-Async` を付けます。

```cs
    public static async void Main()
    {
        // 非同期開始
        var task = this.ReadAsync();

        // 同期的に完了を待てます。
        await task;
    }

    // 非同期メソッドの戻り値はTask, Task<T>とします。
    public Task ReadAsync()
    {
        return Task.Run(() => this.Read());
    }
```

### パラメータ (≒引数)

Camel 形式とします。説明的な名前をつけます。
パラメータ名と型を見ただけで使用法を判断できるような名前が理想的です。
ジェネリックの型パラメータには接頭辞として `T-` をつけます。

- メソッドパラメータ: `Console.WriteLine(string message)`
- 型パラメータ: `Dictionary<TKey, TValue>`

※Parameters は 仮引数を指し, Arguments は 実引数 を指します。

### ローカル変数、ループ変数

ローカル変数 (局所変数) は、Camel 形式とします。

できるだけ最小のスコープ内で、使用する直前に宣言します。
省略した名前はローカル変数以外では禁止とします。
名前を省略した場合でも、できるだけ意味の分かる名前を付けます。
ループ変数は `i, j` とし、それ以上必要な場合は設計を見直します。

- (○) 省略形: `ctor(Constructor), addr(IPAddress), conn(Connection), btn(Button)`
- (○) 意味の分かる省略形: `builder(StringBuilder), reader(StreamReader)`
- (×) 意味の分からない省略形: `sb(StringBuilder), sr(StreamReader)`
- (○) ループ変数: `for(var i = 0; i < count; i++)`

### コンパイル時定数、実行時定数

定数は Pascal 形式とします。

全て大文字で定義するスタイルではないので注意してください。
コンパイル時定数は `const` 、実行時定数は `static readonly` を指します。
`const`はなるべく使用せず、`static readonly`にできないか検討します。

<div class="break" />

### インターフェイス

Pascal 形式とし、クラスの命名規則に従います。

インターフェイスには接頭辞として `I-` をつけ、機能を定義したものには接尾辞として `-able` をつけます。

- 機能を定義したインターフェイス: `interface IDisposable`

### 抽象クラス

Pascal 形式とし、クラスの命名規則に従います。接尾辞として `-Base` をつけます。

- 抽象クラス: `abstract class TextBoxBase`
- TextBoxBase を継承したクラス: `TextBox, RichTextBox`

### デリゲート (≒ 関数ポインタ)

Pascal 形式とし、クラスの命名規則に従います。

デリゲートはメソッドへの参照を保持できるクラスです。
メソッドのシグネチャを指定するため記述が独特となります。
デリゲートには接尾辞として `-Callback` を追加します。
イベント用デリゲートには接尾辞として `-EventHandler` を追加します。

```cs
    delegate void ActionCallback();
    delegate void ActionEventHandler(object sender, EventArgs e);
```

非同期処理を行う場合は、コールバックよりも `async` / `await` の使用を検討します。

### イベント

Pascal 形式とします。イベント名は動詞とします。

時制を表す場合は `-ing` (現在進行形) や `-ed` (過去形) とします。

イベントメソッドは、`On+<イベント名>`とし、引数は `EventArgs` の拡張クラスに限定します。
イベント引数の継承クラスは接尾辞 `EventArgs` を付加します。

```cs
    // イベント用デリゲート変数(<イベント名>)
    // 外部のメソッドを関連付けます。
    public event EventHandler Click;

    // 内部イベント(On+<イベント名>)
    // 何かの処理によりイベントが発生します。
    protected virtual void OnClick(EventArgs e)
    {
        // TODO: 何らかの処理
        // ...

        // 関連する外部メソッドにも通知します。
        this.Click?.Invoke(this, e);
    }
```

### 構造体

Pascal 形式とし、名詞形を用います。

多数の軽量オブジェクトを表す場合に使用します。(例: Color, Point, Rectangle など)
またアンマネージドな DLL と情報をやり取りする場合にも使用します。

以下のすべてに当てはまる場合は構造体の使用を検討します。

* プリミティブ型のように単一で何かをあらわす場合
* インスタンスのサイズが 16Byte 未満
* ボックス化が必要ない場合
* 値が変更されない場合

### 列挙体

列挙体、列挙値ともに Pascal 形式とし、名詞形を用います。

複雑な構造が必要な場合は抽象クラスの使用を検討します。
基本は単数形ですが、`Flags` (ビットフィールド) を表す場合は複数形とします。
