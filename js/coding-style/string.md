## 文字列

文字列にはシングルクオート `''` を使用して下さい。

```js
// bad
const name = "Capt. Janeway";

// bad
const name = `Capt. Janeway`;

// good
const name = 'Capt. Janeway';
```

文字列検索の観点から、長い文字列リテラルであっても見栄えのために文字列連結などで複数にまたがって記述せず、 1 行で記述してください。

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

文字列の一部に変数の値や式の評価結果などを組み込みたい場合、文字列全体をバッククウォート (``` `` ```) で囲み、式を `${}` で囲ってください。
`${}` で式を囲う際には、不要なスペースは入れないでください。

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

可読性を保つために、可能な限り文字列内でのバックスラッシュ (`\`) によるエスケープを避けてください。

```js
// bad
const foo = '\'this\' \i\s \"quoted\"';

// good
const foo = '\'this\' is "quoted"';
const foo = `my name is '${name}'`;
```