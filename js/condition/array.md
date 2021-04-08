## 配列

* 配列を作成する際は、オブジェクトの作成と同じようにリテラル構文を使用して下さい。
* 直接配列に項目を代入せず、Array#pushを使用して下さい。
* 配列をコピーする場合は、`const itemsCopy = [...items];`という風に配列の拡張演算子`...`を使用して下さい。
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
* 繰り返し可能なオブジェクト(iterables)へのマッピングにはスプレッド構文`...`の代わりに`Array.from`を使用すること。理由は中間配列の作成を防ぐためです。
* 配列のコールバック関数の中ではreturn構文を使用すること。もし関数の中が副作用のない式を返す一行で構成されている場合はreturnを省略してもかまいません。
* 配列が複数行ある場合、配列の開始の括弧の後と、最後の括弧の前に改行を入れること。