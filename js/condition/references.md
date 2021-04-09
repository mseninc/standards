## 変数の宣言

変数を宣言する際には `const` を利用します。
どうしても再代入が必要な場合にのみ変数宣言時に `let` を利用します。
いかなる場合も `var` は利用しないでください。

```js
// bad
var a = 1;
var b = 2;

// good
const a = 1;
const b = 2;

// bad
var count = 1;
if (true) {
  count += 1;
}

// good if it is inevitable to reassign
let count = 1;
if (true) {
  count += 1;
}
```
`const` および `let` のスコープの範囲は宣言したブロック内のみです。

```js
// const と let は宣言されたブロックの中でのみ存在します。
const x = 1;
{
  let a = 1;
  const b = 1;
}
console.log(x); // 1
console.log(a); // ReferenceError
console.log(b); // ReferenceError
```
