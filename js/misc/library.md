## 標準ライブラリ

標準ライブラリには非推奨の機能が残っています。

グローバルな `isNaN` の代わりに `Number.isNaN` を使用して下さい。
グローバルな `isNaN` は数字ではないものを数字に強制変換し、NaNに強制変換されたものすべてに `true` を返します。
この動作が望ましい場合は、それを明示してください。

```js
// bad
isNaN('1.2'); // false
isNaN('1.2.3'); // true

// good
Number.isNaN('1.2.3'); // false
Number.isNaN(Number('1.2.3')); // true
```

グローバルな `isFinite` の代わりに `Number.isFinite` を使用して下さい。
グローバルな `isFinite` は数字ではないものを数字に強制変換し、有限数に強制変換されたものすべてに `true` を返します。
この動作が望ましい場合は、それを明示にしてください。

```js
// bad
isFinite('2e3'); // true

// good
Number.isFinite('2e3'); // false
Number.isFinite(parseInt('2e3', 10)); // true
```