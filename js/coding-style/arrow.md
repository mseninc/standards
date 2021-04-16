## アロー関数

無名関数を使用する必要がある場合は、アロー関数を使用してください。
複雑な関数の場合はアロー関数を使用せず、独自に関数宣言してください。

```js
// bad
[1, 2, 3].map(function(x) {
  const y = x + 1;
  return x * y;
});

// good
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});
```

副作用がなく、単一の式の評価結果を返す場合は、中括弧を省略して `return` を省略します。
それ以外の場合は、中括弧を残して `return` 文を使用します。

```js
// bad
[1, 2, 3].map(number => {
  return `A string containing the ${number}.`;
});

// good
[1, 2, 3].map(number => `A string containing the ${number}.`);

// good in case a function is composed of several statements
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

// bad because foo() has side effects
foo(() => bool = true);

// good
foo(() => {
  bool = true;
});
```

式の全長が複数行にまたがる場合は、可読性をより良くするため括弧 `()` で囲ってください。

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

副作用を持たない単一の式の評価結果を返すアロー関数について、関数に与える引数が 1 つの場合にのみ引数の括弧 `()` を省略します。
それ以外の場合は常に括弧で引数を囲みます。

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

アロー関数の関数本体の評価式に比較演算子 (`<=`, `>=`) が含まれる場合は、常に引数を括弧で囲ってください。

```js
// bad
const itemHeight = item => item.height <= 256 ? item.largeSize : item.smallSize;

// bad
const itemHeight = (item) => item.height >= 256 ? item.largeSize : item.smallSize;

// good
const itemHeight = (item) => {
  const { height, largeSize, smallSize } = item;
  return height <= 256 ? largeSize : smallSize;
};
```

アロー関数を使用する場合は、一行に収めるか括弧を用いてスコープを明確にしてください。

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