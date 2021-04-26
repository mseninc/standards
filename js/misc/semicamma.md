# コメント、空白等

## コメント

複数行のコメントには `/** ... */` を使用して下さい。

単一行コメントには `//` を使用して下さい。コメントを加えたいコードの上部に配置して下さい。また、コメントの前に空行を入れてください。

読みやすくするために、すべてのコメントをスペースで始めて下さい。

コメントの前に `FIXME` や `TODO` を付けると、他の開発者が、あなたが再検討が必要な問題を指摘しているのか、あるいは実装が必要な問題の解決策を提案しているのかをすぐに理解することができます。

問題に対する注釈として `// FIXME:` を使用して下さい。

問題の解決策に対する注釈として `// TODO:` を使用して下さい。

```js
    /**
    .....
    */
    
    // comment

    // FIXME: 問題に対する注釈
    // TODO: 問題の解決策
```

## 空白

半角スペース2文字を使用して下さい。

制御構文 (`if` 文や `while` 文など) の丸括弧 (`()`) の前にはスペースを1つ入れて下さい。
関数宣言や関数呼び出し時の引数リストの前にはスペースは入れないで下さい。

```js
// bad
if(isJedi) {
  fight ();
}

// good
if (isJedi) {
  fight();
}

// bad
function fight () {
  console.log ('Swooosh!');
}

// good
function fight() {
  console.log('Swooosh!');
}
```

演算子の間はスペースを入れて下さい。

```js
// bad
const x=y+5;

// good
const x = y + 5;
```

ファイルの最後は改行文字を1つ入れて下さい。

メソッドチェーンが長い場合はインデントを使用して下さい。
メソッドチェーンが続いていることをわかりやすくするために、メソッドチェーンの行頭はドット (`.`) から始めてください。

```js
// bad
$('#items').find('.selected').highlight().end().find('.open').updateCount();

// bad
$('#items').
  find('.selected').
    highlight().
    end().
  find('.open').
    updateCount();

// good
$('#items')
  .find('.selected')
  .highlight()
  .end()
  .find('.open')
  .updateCount();
```

ブロックの直後には、次の文が来る前に改行してください。

```js
// bad
if (foo) {
  return bar;
}
return baz;

// good
if (foo) {
  return bar;
}

return baz;
```

ブロックに空行を挟み込まないでください。

```js
// bad
function bar() {

  console.log(foo);

}

// good
function bar() {
  console.log(foo);
}

// bad
if (baz) {

  console.log(qux);
} else {
  console.log(foo);

}

// good
if (baz) {
  console.log(qux);
} else {
  console.log(foo);
}
```

丸括弧 (`()`) と角括弧 (`[]`) の内側にはスペースを追加しないでください。

```js
// bad
if ( foo ) {
  console.log(foo);
}

// good
if (foo) {
  console.log(foo);
}

// bad
const foo = [ 1, 2, 3 ];
console.log(foo[ 0 ]);

// good
const foo = [1, 2, 3];
console.log(foo[0]);
```

中括弧 (`{}`) の内側にはスペースを追加してください。

```js
// bad
const foo = {clark: 'kent'};

// good
const foo = { clark: 'kent' };
```

100文字を超えるコード行 (空白を含む) を避けて下さい。
ただし長い文字列は改行で分割しないでください。

開いている中括弧 (`{`) と同じ行の次の中括弧 (`}`) の間に一定のスペースを空けて下さい。
この規則は、同じ行の閉じ中括弧 (`}`) と前の中括弧 (`{`) との間に一貫性のあるスペースを適用します。

カンマの前にスペースを入れず、カンマの後にスペースを入れて下さい。

```js
// bad
const arr = [1 , 2];

// good
const arr = [1, 2];
```

関数呼び出しにおいて、関数名と引数の括弧の間にスペースを入れないでください。

```js
// bad
func ();

func
();

// good
func();
```

オブジェクトリテラルプロパティのキーと値の間のスペースを入れて下さい。

```js
// bad
var obj = { foo : 42 };
var obj2 = { foo:42 };

// good
var obj = { foo: 42 };
```

行末に末尾のスペースを入れないで下さい。
複数の空行を避け、ファイルの終わりには改行を1つだけ入れて下さい。

## カンマ

カンマは先頭につけず、末尾につけて下さい。

```js
// bad
const hero = {
    firstName: 'Ada'
  , lastName: 'Lovelace'
  , birthYear: 1815
  , superPower: 'computers'
};

// good
const hero = {
  firstName: 'Ada',
  lastName: 'Lovelace',
  birthYear: 1815,
  superPower: 'computers',
};
```

## セミコロン

文には必ずセミコロン (`;`) をつけて下さい。

```js
// bad
function foo() {
  return 'search your feelings, you know it to be foo'
}

// good
function foo() {
  return 'search your feelings, you know it to be foo';
}
```