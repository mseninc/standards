## アロー関数

* 無名関数を使用する必要がある場合は、アロー関数表記を使用してください。アロー関数はそのコンテキストの`this`で実行するバージョンの関数を作成します。これは通常期待通りの動作をし、より簡潔な構文だからです。
複雑な関数でロジックを定義した関数の外側に移動したい場合は使用しないで下さい。
```js
// bad
[1, 2, 3].map(function (x) {
  const y = x + 1;
  return x * y;
});

// good
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});
```
* 関数本体が、副作用のない式を返す1つのステートメントで構成されている場合は、中括弧を省略して`return`を省略します。それ以外の場合は、中括弧を残して`return`文を使用します。
```js
// bad
[1, 2, 3].map(number => {
  const nextNumber = number + 1;
  `A string containing the ${nextNumber}.`;
});

// good
[1, 2, 3].map(number => `A string containing the ${number}.`);

// good
[1, 2, 3].map((number) => {
  const nextNumber = number + 1;
  return `A string containing the ${nextNumber}.`;
});

// good
[1, 2, 3].map((number, index) => ({
  [index]: number,
}));

// No implicit return with side effects
function foo(callback) {
  const val = callback();
  if (val === true) {
    // Do something if callback returns true
  }
}

let bool = false;

// bad
foo(() => bool = true);

// good
foo(() => {
  bool = true;
});
```
* 式の全長が複数行にまたがる場合は、可読性をより良くするため丸括弧`()`で囲うこと。関数の開始と終了部分が分かりやすく見るためです。
```js
// bad
['get', 'post', 'put'].map(httpMethod => Object.prototype.hasOwnProperty.call(
    httpMagicObjectWithAVeryLongName,
    httpMethod,
  )
);

// good
['get', 'post', 'put'].map(httpMethod => (
  Object.prototype.hasOwnProperty.call(
    httpMagicObjectWithAVeryLongName,
    httpMethod,
  )
));
```
* 関数が単一の引数を取り、中括弧を使用しない場合は、括弧を省略すること。 それ以外の場合は、明確さと一貫性を保つために、引数を常に括弧で囲むこと。常に括弧を使用する場合は、eslintに“always”オプションを使用すること。
```js
// bad
[1, 2, 3].map((x) => x * x);

// good
[1, 2, 3].map(x => x * x);

// good
[1, 2, 3].map(number => (
  `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
));

// bad
[1, 2, 3].map(x => {
  const y = x + 1;
  return x * y;
});

// good
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});
```
* アロー関数の構文(`=>`)と比較演算子(`<=`、`>=`)を混同しない。
```js
// bad
const itemHeight = item => item.height <= 256 ? item.largeSize : item.smallSize;

// bad
const itemHeight = (item) => item.height >= 256 ? item.largeSize : item.smallSize;

// good
const itemHeight = item => (item.height <= 256 ? item.largeSize : item.smallSize);

// good
const itemHeight = (item) => {
  const { height, largeSize, smallSize } = item;
  return height <= 256 ? largeSize : smallSize;
};
```
* アロー関数を使用する場合は、一行に収めるか括弧を用いてスコープを明確にする。
```js
// bad
foo =>
  bar;

foo =>
  (bar);

// good
foo => bar;
foo => (bar);
foo => (
   bar
)
```