## 比較演算子と同等性

* `if`文のような条件式は`ToBoolean`メソッドによる強制型変換で評価され、常に以下のルールに従います。

+ オブジェクトはtrueと評価されます。
+ undefinedはfalseと評価されます。
+ nullはfalseと評価されます。
+ 真偽値はboolean型の値として評価されます。
+ 数値はtrueと評価されます。しかし、+0, -0, NaNの場合は falseです。
+ 文字列はtrueと評価されます。しかし、空文字`''`の場合はfalse です。
```js
if ([0]) {
  // 配列はオブジェクトなのでtrueとして評価されます。
}
```

* 真偽値にはショートカットを使用しますが、文字列と数値には明示的な比較を使用すること。
```js
// bad
if (isValid === true) {
  // ...
}

// good
if (isValid) {
  // ...
}

// bad
if (name) {
  // ...
}

// good
if (name !== '') {
  // ...
}

// bad
if (collection.length) {
  // ...
}

// good
if (collection.length > 0) {
  // ...
}
```
* `case`と`default`節でブロックを作成するには中括弧(`{}`)を使うこと。(例えば `let`、`const`、`function`、`class`) レキシカル宣言は`switch`ブロック全体から参照することができますが、代入されたときにだけ初期化され、それは`case`に達したときにだけ起こります。複数の`case`節が同じものを定義しようとするとき、これは問題を引き起こします。
```js
// bad
switch (foo) {
  case 1:
    let x = 1;
    break;
  case 2:
    const y = 2;
    break;
  case 3:
    function f() {
      // ...
    }
    break;
  default:
    class C {}
}

// good
switch (foo) {
  case 1: {
    let x = 1;
    break;
  }
  case 2: {
    const y = 2;
    break;
  }
  case 3: {
    function f() {
      // ...
    }
    break;
  }
  case 4:
    bar();
    break;
  default: {
    class C {}
  }
}
```
* 三項演算子は入れ子にするべきではなく、一般に単一行にします。
```js
// bad
const foo = maybe1 > maybe2
  ? "bar"
  : value1 > value2 ? "baz" : null;

// split into 2 separated ternary expressions
const maybeNull = value1 > value2 ? 'baz' : null;

// better
const foo = maybe1 > maybe2
  ? 'bar'
  : maybeNull;

// best
const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
```
* 不要な三項演算子を避ける。
```js
// bad
const foo = a ? a : b;
const bar = c ? true : false;
const baz = c ? false : true;

// good
const foo = a || b;
const bar = !!c;
const baz = !c;
```
* 演算子を混在させる場合は括弧で囲むこと。唯一の例外は、標準の算術演算子（`+`、`-`、`*`、`/`）です。これにより読みやすさが向上し、開発者の意図が明確になります。
```js
// bad
const foo = a && b < 0 || c > 0 || d + 1 === 0;

// bad
const bar = a ** b - 5 % d;

// bad
// (a || b) && c だと考えて混乱するかもしれません。
if (a || b && c) {
  return d;
}

// good
const foo = (a && b < 0) || c > 0 || (d + 1 === 0);

// good
const bar = (a ** b) - (5 % d);

// good
if (a || (b && c)) {
  return d;
}

// good
const bar = a + b / c * d;
```