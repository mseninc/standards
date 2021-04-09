## 文字列

* 文字列にはシングルクオート `''` を使用して下さい。
```js
// bad
const name = "Capt. Janeway";

// bad
const name = `Capt. Janeway`;

// good
const name = 'Capt. Janeway';
```
* 100文字以上になるような文字列は、文字列連結を使用して複数行にまたがって記述しない。
作業するうえで負担になる上にコードの検索容易性を損ないます。
```js
// bad
const errorMessage = 'This is a super long error that was thrown because \
of Batman. When you stop to think about how Batman had anything to do \
with this, you would get nowhere \
fast.';

// bad
const errorMessage = 'This is a super long error that was thrown because ' +
  'of Batman. When you stop to think about how Batman had anything to do ' +
  'with this, you would get nowhere fast.';

// good
const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
```
* プログラムで文字列を生成する場合は、文字列連結ではなく、テンプレート文字列を使用すること。テンプレート文字列は文字列補完機能・複数行文字列機能を持つ簡潔な構文で、可読性が良いからです。
```js
// bad
function sayHi(name) {
  return 'How are you, ' + name + '?';
}

// bad
function sayHi(name) {
  return ['How are you, ', name, '?'].join();
}

// bad
function sayHi(name) {
  return `How are you, ${ name }?`;
}

// good
function sayHi(name) {
  return `How are you, ${name}?`;
}
```
* 絶対に`eval()`を利用しない。脆弱性の原因になります。
* 文字列の中で文字を不必要にエスケープしない。バックスラッシュは可読性を損なうため、必要なときにだけ存在させるべきです。
```js
// bad
const foo = '\'this\' \i\s \"quoted\"';

// good
const foo = '\'this\' is "quoted"';
const foo = `my name is '${name}'`;
```