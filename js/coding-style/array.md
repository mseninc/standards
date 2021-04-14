## 配列

* 配列を作成する際は、オブジェクトの作成と同じようにリテラル構文を使用して下さい。
```js
// bad
const items = new Array();

// good
const items = [];
```
* 直接配列に項目を代入せず、Array#pushを使用して下さい。
```js
const someStack = [];

// bad
someStack[someStack.length] = 'abracadabra';

// good
someStack.push('abracadabra');
```
* 配列をコピーする場合は、`const itemsCopy = [...items];`という風に配列の拡張演算子`...`を使用して下さい。
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
* 繰り返し可能なオブジェクトを配列に変換する場合は、
```js
const foo = document.querySelectorAll('.foo');

// good
const nodes = Array.from(foo);

// best
const nodes = [...foo];
```
という風に`Array.from`の代わりにスプレッド構文`...`を使用して下さい。
* array-likeオブジェクトを配列に変換する場合は、
`const arr = Array.prototype.slice.call(arrLike);`とするのではなく、
`const arr = Array.from(arrLike);`という風に`Array.from`を使用すること。
* 中間配列の作成を避けるため、反復可能なデータのマッピングには スプレッド構文 `...` ではなく、`Array.from` を使用してください。
```js
// bad
const baz = [...foo].map(bar);

// good
const baz = Array.from(foo, bar);
```
* 配列のコールバック関数の中ではreturn構文を使用すること。もし関数の中が副作用のない式を返す一行で構成されている場合はreturnを省略してもかまいません。
```js
// good
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});

// good
[1, 2, 3].map(x => x + 1);

// bad - 値を返さないために`acc`は最初の繰り返しのあとでundefinedになります。
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
  if (subject === 'Mockingbird') {
    return author === 'Harper Lee';
  }

  return false;
});
```
* 配列が複数行ある場合、配列の開始の括弧の後と、最後の括弧の前に改行を入れること。
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