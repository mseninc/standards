## 制御ステートメント

* 制御文（`if`、`while`など）が長くなり過ぎたり、最大行長を超えたりした場合、それぞれの（グループ化された）条件は新しい行に入れること。そして論理演算子で行を始めること。行の先頭で演算子を要求すると、演算子は整列されたままになり、メソッド連結と同様のパターンに従います。これにより、複雑なロジックを視覚的に理解しやすくなるため、読みやすくなります。
```js
// bad
if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
  thing1();
}

// bad
if (foo === 123 &&
  bar === 'abc') {
  thing1();
}

// bad
if (foo === 123
  && bar === 'abc') {
  thing1();
}

// bad
if (
  foo === 123 &&
  bar === 'abc'
) {
  thing1();
}

// good
if (
  foo === 123
  && bar === 'abc'
) {
  thing1();
}

// good
if (
  (foo === 123 || bar === 'abc')
  && doesItLookGoodWhenItBecomesThatLong()
  && isThisReallyHappening()
) {
  thing1();
}

// good
if (foo === 123 && bar === 'abc') {
  thing1();
}
```
* 後続処理を選択するための演算子を制御文の代わりに使用しない。
```js
// bad
!isRunning && startRunning();

// good
if (!isRunning) {
  startRunning();
}
```