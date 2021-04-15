## 関数

即時関数の実行を記述する際には、関数定義全体を括弧で囲ってください。

```js
(function () {
  console.log('Welcome to the Internet. Please follow me.');
})();
```

制御構文などのブロック内で関数定義を記述しないでください。
代わりに、アロー関数式を変数に代入して使用してください。

```js
// bad
if (currentUser) {
  function test() {
    console.log('Nope.');
  }
}

// good
let test;
if (currentUser) {
  test = () => {
    console.log('Yup.');
  };
}
```

関数スコープに与えられている `arguments` オブジェクトが上書きされるため、関数の引数名に `arguments` を指定しないでください。

```js
// bad
function foo(name, options, arguments) {
  // ...
}

// good
function foo(name, options, args) {
  // ...
}
```

可変引数の引数には `arguments`を使用せず、 `...` 演算子を使用してください。

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

関数内で引数の値を変更しないでください。
引数に初期値を与える場合はデフォルト引数を使用してください。

```js
// really bad
function handleThings(opts) {
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

副作用のあるデフォルト引数は使用しないでください。

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

引数が複数ある場合、デフォルト引数は末尾に指定してください。

```js
// bad
function handleThings(name, opts = {}, age)
{
  // do something...
}

// good
function handleThings(name, age, opts = {})
{
  // do something...
}
```

関数定義に `Function` コンストラクタを使用しないでください。

```js
// bad
const add = new Function('a', 'b', 'return a + b');

const subtract = Function('a', 'b', 'return a - b');

// good
const add = function(a, b) {
  return a + b;
}

const subtract = (a, b) => a - b;
```

関数定義の引数の括弧と定義部の波括弧の間にはスペースを入れてください。
関数名と括弧、および無名関数の場合は `function` と括弧の間にはスペースを入れないでください。

```js
// bad
const f = function(){};
const g = function (){};
const h = function() {};

// good
const x = function() {};
const y = function a() {};
```

予期しない副作用を防ぐために、引数に渡されたオブジェクトのプロパティなどを直接操作しないでください。

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

可変引数の関数を呼び出す場合はスプレッド演算子 (`...`) を使用してください。

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

複数行の関数構文や呼び出しではインデントしてください。
最後の項目に末尾のコンマを付けてください。

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
