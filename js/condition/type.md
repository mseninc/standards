## 型

* プリミティブ型は値を直接操作して下さい。
また、参照型は参照を通して値を操作して下さい。
```js
const foo = 1;
let bar = foo;

bar = 9;

console.log(foo, bar); // => 1, 9

const foo = [1, 2];
const bar = foo;

bar[0] = 9;

console.log(foo[0], bar[0]); // => 9, 9
```