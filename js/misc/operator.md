## 演算子

`if` 文の条件式で変数を評価する場合、文字列と数値に関しては明示的に比較演算を行ってください。
それら以外に関して `true` または `false` を評価する際は、等価演算子を省略してください。

```js
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

// bad
if (isValid === true) {
  // ...
}

// good
if (isValid) {
  // ...
}
```

三項演算子の入れ子を避け、単一行で記述してください。

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

不要な三項演算子を避けてください。

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

`&&` と `||` などの演算子が混在する場合、演算順序を明確にするために式を括弧で囲ってください。
ただし、 `+` と `-`, `*` と `/` のように演算順序が同列の算術演算に関しては括弧を省略しても構いません。

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

// bad
const bar = a + b / c * d;

// good
const foo = (a && b < 0) || c > 0 || (d + 1 === 0);

// good
const bar = (a ** b) - (5 % d);

// good
if (a || (b && c)) {
  return d;
}

// good
const bar = a + (b / c * d);
```
