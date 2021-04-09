## 関数

* 関数宣言ではなく名前付き関数式を使用すること。関数の定義は巻き上げられるため、読みやすさや保守性を損ないます。関数の定義が十分に大きいか複雑であることがわかった場合、モジュールに展開するようにして下さい。
名前が定義されている変数から推測されるかどうかにかかわらず、式に明示的に名前を付けることを忘れないでください。
```js
// bad
function foo() {
  // ...
}

// bad
const foo = function () {
  // ...
};

// good
const short = function longUniqueMoreDescriptiveLexicalFoo() {
  // ...
};
```
* 即時呼び出し関数式(IIFE)を括弧で囲むこと。即時関数式は単一の単位であり、その両方とその呼び出し括弧を括弧で囲んで明確に表します。モジュールがある場合は必要ありません。
```js
(function () {
  console.log('Welcome to the Internet. Please follow me.');
}());
```
* 関数ではないブロック(`if`, `while`, など)の中で関数を定義しない。代わりに変数に関数を割り当てること。ブラウザは実行を許可しますが、すべて違ったように解釈されます。
* パラメータに`arguments`を指定しないで下さい。関数スコープに渡される`arguments`オブジェクトの参照を上書きしてしまいます。
* `arguments`を利用しない。代わりにrest syntax`...`を使用すること。`...`を利用することで、いつくかのパラメータを利用したいことを明らかにすることができます。加えてrestパラメータは`arguments`の様なArray-likeなオブジェクトではなく正真正銘のArrayです。
```js
// bad
function concatenateAll() {
  const args = Array.prototype.slice.call(arguments);
  return args.join('');
}

// good
function concatenateAll(...args) {
  return args.join('');
}
```
* 関数のパラメータを突然変異させるのではなく、デフォルトパラメータを使用すること。
```js
// really bad
function handleThings(opts) {
  // 関数のパラメータを勝手に変化させてはいけません。
  opts = opts || {};
  // ...
}

// still bad
function handleThings(opts) {
  if (opts === void 0) {
    opts = {};
  }
  // ...
}

// good
function handleThings(opts = {}) {
  // ...
}
```
* 副作用のあるデフォルトパラメータの利用を避ける。
```js
var b = 1;
// bad
function count(a = b++) {
  console.log(a);
}
count();  // 1
count();  // 2
count(3); // 3
count();  // 3
```
* `function handleThings(name, opts = {})`という風に常にデフォルトパラメータは末尾に配置すること。
* 新しい関数を作成するためにFunctionコンストラクタを使用しない。この方法で文字列を評価させて新しい関数を作成することは、eval()と同様の脆弱性を引き起こします。
```js
// bad
var add = new Function('a', 'b', 'return a + b');

// still bad
var subtract = Function('a', 'b', 'return a - b');
```
* 関数構文の中にスペースを入れること。
```js
// bad
const f = function(){};
const g = function (){};
const h = function() {};

// good
const x = function () {};
const y = function a() {};
```
* パラメータを直接操作しないこと。パラメータに渡されたオブジェクトを操作することは、呼び出し元にて望まれない値の副作用を及ぼす可能性があります。
```js
// bad
function f1(obj) {
  obj.key = 1;
}

// good
function f2(obj) {
  const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
}
```
* パラメータを再割り当てしない。特に`arguments`オブジェクトにアクセスするとき、パラメータを再割り当てすると予期しない動作をする可能性があります。 特にV8では最適化の問題も発生する可能性があります。
```js
// bad
function f1(a) {
  a = 1;
  // ...
}

function f2(a) {
  if (!a) { a = 1; }
  // ...
}

// good
function f3(a) {
  const b = a || 1;
  // ...
}

function f4(a = 1) {
  // ...
}
```
* 可変引数の関数を呼び出す場合はスプレッド演算子`...`を使用すること。
```js
// bad
const x = [1, 2, 3, 4, 5];
console.log.apply(console, x);

// good
const x = [1, 2, 3, 4, 5];
console.log(...x);

// bad
new (Function.prototype.bind.apply(Date, [null, 2016, 8, 5]));

// good
new Date(...[2016, 8, 5]);
```
* 複数行の関数構文や呼び出しではインデントして下さい。最後の項目に末尾のコンマを付けて、行の各項目を単独で指定すること。
```js
// bad
function foo(bar,
             baz,
             quux) {
  // ...
}

// good
function foo(
  bar,
  baz,
  quux,
) {
  // ...
}

// bad
console.log(foo,
  bar,
  baz);

// good
console.log(
  foo,
  bar,
  baz,
);
```