## オブジェクト

* オブジェクトを作成する際は、`const item = new Object();`とせず、`const item = {};`という風にリテラル構文を使用して下さい。
* 動的にプロパティ名を持つオブジェクトを作成する場合、
```js
const obj = {
  id: 5,
  name: 'San Francisco',
};
obj[getKey('enabled')] = true;
```
とせず、
```js
const obj = {
  id: 5,
  name: 'San Francisco',
  [getKey('enabled')]: true,
};
```
として下さい。
* メソッドやプロパティの記述は短縮構文を使用して下さい。
```js
// bad
const atom = {
  value: 1,

  addValue: function (value) {
    return atom.value + value;
  },
};

// good
const atom = {
  value: 1,

  addValue(value) {
    return atom.value + value;
  },
};
```
* プロパティの短縮構文はオブジェクト宣言の先頭にまとめて下さい。
どのプロパティが短縮構文を利用しているか分かりやすいからです。
```js
const lukeSkywalker = 'Luke Skywalker';

// bad
const obj = {
  lukeSkywalker: lukeSkywalker,
};

// good
const obj = {
  lukeSkywalker,
};
```
* 無効な識別子の場合のみプロパティを引用符で括ること。
一般的にこちらの方が読みやすいと考えてられています。これは構文の強調表示を改善し、また多くのJSエンジンによってより簡単に最適化されます。
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
* `hasOwnProperty`、`propertyIsEnumerable`、`isPrototypeOf`のような`Object.prototype`の関数を直接呼び出さないで下さい。
```js
// bad
console.log(object.hasOwnProperty(key));

// good
console.log(Object.prototype.hasOwnProperty.call(object, key));

// best
const has = Object.prototype.hasOwnProperty; // cache the lookup once, in module scope.
/* or */
import has from 'has'; // https://www.npmjs.com/package/has
// ...
console.log(has.call(object, key));
```
* 無効な識別子の場合のみプロパティを引用符で括ること。
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
* オブジェクトをシャローコピーする場合は`Object.assign`よりもオブジェクトスプレッド構文を使用すること。特定のプロパティを省略した新しいオブジェクトを取得するには、オブジェクトのレスト演算子を使用すること。
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