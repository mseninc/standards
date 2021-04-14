## オブジェクト

* オブジェクトを作成する際は、オブジェクトのリテラル構文 (`{}`) を使用して下さい。

```js
// bad
const item = new Object();

// good
const item = {};
```

* 動的にプロパティ名を持つオブジェクトを作成する場合、

動的なプロパティ名を持つオブジェクトを生成する際には、そのプロパティ名を生成する式を `[]` で囲ってキーとしてください。

```js
// bad
const obj = {
  id: 5,
  name: 'San Francisco',
};
obj[getKey('enabled')] = true;

// good
const obj = {
  id: 5,
  name: 'San Francisco',
  [getKey('enabled')]: true,
};
```

メソッドやプロパティの記述は短縮構文を使用して下さい。
プロパティ名と値を格納する変数名が同じ場合はプロパティ名のみを記述します。
メソッドを定義する場合は、プロパティの値に関数を指定するのではなく、 `メソッド名(引数){ 文 }` のように定義してください。

```js
// bad
const value = 1;
const atom = {
  value: value,
  addValue: function (value) {
    return atom.value + value;
  },
};

// good
const atom = {
  value,
  addValue(value) {
    return atom.value + value;
  },
};
```

プロパティの短縮構文はオブジェクト宣言の先頭にまとめて下さい。
どのプロパティが短縮構文を利用しているか分かりやすいからです。

```js
const anakinSkywalker = 'Anakin Skywalker';
const lukeSkywalker = 'Luke Skywalker';

// bad
const obj = {
  episodeOne: 1,
  twoJediWalkIntoACantina: 2,
  lukeSkywalker,
  episodeThree: 3,
  mayTheFourth: 4,
  anakinSkywalker,
};

// good
const obj = {
  lukeSkywalker,
  anakinSkywalker,
  episodeOne: 1,
  twoJediWalkIntoACantina: 2,
  episodeThree: 3,
  mayTheFourth: 4,
};
```

変数名として無効なプロパティ名を指定する場合のみ、プロパティを引用符で括ってください。

```js
// bad
const bad = {
  'foo': 3,
  'bar': 4,
  'data-blah': 5,
};

// good
const good = {
  foo: 3,
  bar: 4,
  'data-blah': 5,
};
```

`hasOwnProperty`, `propertyIsEnumerable`, `isPrototypeOf` のような `Object.prototype` のメソッドをインスタンスから直接起動しないでください。

```js
// bad
console.log(obj.hasOwnProperty(key));

// good
console.log(Object.prototype.hasOwnProperty.call(object, key));

// best
const has = Object.prototype.hasOwnProperty; // cache the lookup once, in module scope.
/* or */
import has from 'has'; // https://www.npmjs.com/package/has

console.log(has.call(obj, key));
```

オブジェクトをシャローコピーする場合はスプレッド演算子を使用してください。
特定のプロパティを省略した新しいオブジェクトを取得するには、オブジェクトのレスト演算子 (`...`) を使用すること。
<!-- [NOTES]
... 演算子が文脈によって「スプレッド演算子」と呼ぶべき場合と「レスト演算子」と呼ぶべき場合が存在する。
同じ演算子なので名称をできれば統一したいが、すべての場合で「スプレッド演算子」と名付けてしまうのは少し乱暴。
どうすべきか検討する。
-->

```js
// very bad
const original = { a: 1, b: 2 };
const copy = Object.assign(original, { c: 3 }); // this mutates `original` ಠ_ಠ
delete copy.a; // so does this

// bad
const original = { a: 1, b: 2 };
const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

// good
const original = { a: 1, b: 2 };
const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
```