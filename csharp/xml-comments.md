## XML コメント

.NET ではクラスやメンバーなどに XML コメントを記述することで、下記のような効果が期待できます。

- IntelliSense による開発の効率化
- ドキュメントの自動生成

プロダクトコードの公開メンバーについては **必ず** XML コメントを記述します。

## コメント記述の例

### クラス

#### ViewModel

```cs
/// <summary>
/// <see cref="View クラス"> に対する ViewModel です。
/// </summary>
```

Base クラスなどで、View のクラス名が直接存在しない場合は、 `<see cref="具体的な View クラス"> などの○○を表すビューに対する ViewModel です。` のような表現にします。

### プロパティ

#### アクセスの種類による末尾表現

`set` が公開 (public, protected) されている通常のプロパティ、もしくは ReactiveProperty (`ReactiveProperty`, `ReactivePropertySlim`) の場合は、内容のあとに「を設定または取得します。」をつけます。

```cs
/// <summary>
/// ○○を設定または取得します。
/// </summary>
```

`set` が非公開もしくは `get` のみの場合、あるいは `ReadOnlyReactiveProperty`, `ReadOnlyReactivePropertySlim` の場合は、内容のあとに「を取得します。」をつけます。

```cs
/// <summary>
/// ○○を取得します。
/// </summary>
```

#### 型による表現

##### boolean

boolean の場合は「かどうか」をつけます。

```cs
/// <summary>
/// ○○かどうかを設定または取得します。
/// </summary>
```
```cs
/// <summary>
/// ○○かどうかを取得します。
/// </summary>
```


### メソッド

そのメソッドの **外的なふるまい** を端的な文章で表します。可能な限り引数がどのような役割を果たすかの説明を加えてください。

内部実装についての説明が必要な場合は `<remarks>` に記述します。

```cs
/// <summary>
/// 指定したファイルから書籍の一覧を読み込み、指定した顧客が借りている書籍を取得します。
/// </summary>
/// <param name="fileName">ファイル名</param>
/// <param name="customerName">顧客の名前</param>
/// <returns>書籍のリストを返す <see cref="Task"/></returns>
/// <exception cref="FileNotFoundException">ファイルが見つかりません。</exception>
/// <remarks>
/// このメソッドは内部的に○○を呼び出します。
/// </remarks>
public Task<List<Book>> GetCurrentlyRentedBooks(string fileName, string customerName)
{

}
```

### メソッドの引数（パラメーター）

名詞、名詞句、または体言止めで記述します。

```cs
/// <param name="fileName">ファイル名</param>
/// <param name="customerName">顧客の名前</param>
/// <param name="cancellationToken">処理をキャンセルするための <see cref="CancellationToken"/></param>
```

型名があるほうが理解しやすい場合は `<see>` タグで参照します。

### メソッドの戻り値

名詞、名詞句、または体言止めで記述します。

#### 条件によって戻り値が異なる場合

```cs
/// <returns>○○の場合は△△、それ以外は▲▲</returns>
```

#### 非同期メソッド

```cs
/// <returns><see cref="Task"/></returns> 👈 Task の場合
/// <returns>○○を返す <see cref="Task"/></returns> 👈 Task<T> の場合
```

### メソッドの例外

そのメソッドが**例外をスローする場合は、 `<exception>` を記述**します。

メソッド内で、例外を返す可能性のあるメソッドを呼び出している場合は、可能な限りその例外情報も記述します。

```cs
/// <exception cref="FileNotFoundException">ファイルが見つかりません。</exception>
/// <exception cref="ArgumentException">○○に空文字を指定することはできません。</exception>
```
