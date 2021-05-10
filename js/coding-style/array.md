## 配列

配列を作成する際は、配列のリテラル構文 (`[]`) を使用して下さい。

```js
// bad
const items = new Array();

// good
const items = [];
```

配列の要素を追加する場合は `Array#push()` を使用してください。

```js
const someStack = [];

// bad
someStack[someStack.length] = 'abracadabra';

// good
someStack.push('abracadabra');
```

配列をコピーする場合はスプレッド構文 (`...`) を使用してください。

```js
// bad
const len = items.length;
const itemsCopy = [];
let i;

for (i = 0; i < len; i++) {
  itemsCopy[i] = items[i];
}

// good
const itemsCopy = [...items];
```

繰り返し可能なオブジェクトを配列に変換する場合は、 `Array.from()` の代わりにスプレッド演算子 (`...`) を使用してください。

```js
const foo = document.querySelectorAll('.foo');

// good
const nodes = Array.from(foo);

// best
const nodes = [...foo];
```

Array-like なオブジェクトを配列に変換する際には、 `Array.from()` を使用してください。

```js
// bad
const arr = Array.prototype.slice.call(arrLike);

// good
const arr = Array.from(arrLike);
```

列挙可能なオブジェクトを `map()` などで変換する場合は、 一時的な配列が生成されることを防ぐために `Array.from()` を使用してください。

```js
// bad
const result = [...iterable].map(x => f(x));

// good
const result = Array.from(iterable, x => f(x));
```

配列のコールバック関数の中では `return` 構文を使用してください。
ただし、関数が副作用を持たない、単一の式を返す場合は `return` を省略します。

```js
// good
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});

// good
[1, 2, 3].map(x => x + 1);

// bad - 値を返さないために acc は最初の繰り返しのあとで undefined になります。
[[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
  const flatten = acc.concat(item);
  acc[index] = flatten;
});

// good
[[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
  const flatten = acc.concat(item);
  acc[index] = flatten;
  return flatten;
});

// bad
inbox.filter((msg) => {
  const { subject, author } = msg;
  if (subject === 'Mockingbird') {
    return author === 'Harper Lee';
  } else {
    return false;
  }
});

// good
inbox.filter((msg) => {
  const { subject, author } = msg;

  return false;
});
```

配列が複数行ある場合、配列の開始の括弧の後と、最後の括弧の前に改行を入れてください。
また、改行する場合はすべての配列の要素の最後にカンマ (`,`) を入れてください。

```js
// bad
const arr = [
  [0, 1], [2, 3], [4, 5],
];

const objectInArray = [{
  id: 1,
}, {
  id: 2,
}];

const numberInArray = [
  1, 2,
];

// good
const arr = [[0, 1], [2, 3], [4, 5]];

const objectInArray = [
  {
    id: 1,
  },
  {
    id: 2,
  },
];

const numberInArray = [
  1,
  2,
];
```