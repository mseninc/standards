## 参照

* すべての参照は`const`を使用し、`var`を使用しないで下さい。参照を再割り当てできないことで、バグに繋がりやすく理解しがたいコードになることを防ぎます。また、参照を再割当てする場合は`var`の代わりに`let`を使用して下さい。`let`は`var`のように関数スコープではなくブロックスコープです。
`let`と`const`はブロックスコープであることに注意して下さい。
```js
// bad
var a = 1;
var b = 2;

// good
const a = 1;
const b = 2;
```
```js
// bad
var count = 1;
if (true) {
  count += 1;
}

// good, use the let.
let count = 1;
if (true) {
  count += 1;
}
```
```js
// const と let は宣言されたブロックの中でのみ存在します。
{
  let a = 1;
  const b = 1;
}
console.log(a); // ReferenceError
console.log(b); // ReferenceError
```