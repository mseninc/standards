## プロパティ

* プロパティにアクセスする場合はドット(`.`)を使用すること。
```js
const luke = {
  jedi: true,
  age: 28,
};

// bad
const isJedi = luke['jedi'];

// good
const isJedi = luke.jedi;
```
* 変数を使用してプロパティにアクセスする場合は角括弧(`[]`)を使用すること
```js
const luke = {
  jedi: true,
  age: 28,
};

function getProp(prop) {
  return luke[prop];
}

const isJedi = getProp('jedi');
```
* べき乗を計算するときは、べき乗演算子`**`を使用すること。
```js
// bad
const binary = Math.pow(2, 10);

// good
const binary = 2 ** 10;
```