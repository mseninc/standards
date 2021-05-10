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

代入文において `=` の前後に改行を入れないでください。
代入がコーディングルール `max-len` に違反している場合は、値を丸括弧()で囲んでください。

```js
// bad
const foo =
  superLongLongLongLongLongLongLongLongFunctionName();

// bad
const foo
  = 'superLongLongLongLongLongLongLongLongString';

// good
const foo = (
  superLongLongLongLongLongLongLongLongFunctionName()
);

// good
const foo = 'superLongLongLongLongLongLongLongLongString';
```

1 つの変数宣言に対して 1 つの `const` または `let` を利用してください。

```js
// bad
const items = getItems(),
    goSportsTeam = true,
    dragonball = 'z';

// good
const items = getItems();
const goSportsTeam = true;
const dragonball = 'z';
```

`const` で宣言される変数と `let` で宣言される変数をそれぞれ 1 つのグループとして集約して宣言してください。

```js
// bad
let i;
const items = getItems();
let dragonball;
const goSportsTeam = true;
let len;

// good
const goSportsTeam = true;
const items = getItems();
let dragonball;
let i;
let length;
```

変数の宣言は、必要最小限のスコープで使用されるように宣言してください。

```js
// bad
let additionalMessage;
if (today.isHoliday()) {
  if (me.isFine()) {
    additionalMessage = `and ${me.name} is fine.`;
  } else {
    additionalMessage = `but ${me.name} is not good.`;
  }
  console.log(`It's holiday ${additionalMessage}`);
}

// good
if (today.isHoliday()) {
  let additionalMessage;
  if (me.isFine()) {
    additionalMessage = `and ${me.name} is fine.`;
  } else {
    additionalMessage = `but ${me.name} is not good.`;
  }
  console.log(`It's holiday ${additionalMessage}`);
}
```

可能な限り最初に参照される直前に変数を宣言するようにしてください。

```js
// bad
function checkName(hasName) {
  // `name` is unnecessary the following first if statement.
  const name = getName();

  if (hasName === 'test') {
    return false;
  }

  if (name === 'test') {
    this.setName('');
    return false;
  }

  return name;
}

// good
function checkName(hasName) {
  if (hasName === 'test') {
    return false;
  }

  // `name` is used the following if statement.
  const name = getName();
  if (name === 'test') {
    this.setName('');
    return false;
  }

  return name;
}
```

不要な変数を宣言しないでください。
ただし、 `...` 演算子を用いて特定のプロパティを除いたオブジェクトを宣言する場合はこの限りではありません。

```js
// bad
const some_unused_var = 42;

// 関数の引数の一部が未使用になっています。
function getX(x, y) {
    return x;
}

// good
function getXPlusY(x, y) {
  return x + y;
}

const x = 1;
const y = a + 2;

alert(getXPlusY(x, y));

// 'type' は rest プロパティで 'coords' から除外する要素なので、未使用でも問題ありません。
const { type, ...coords } = data;
// 'coords' は、 'type' プロパティを持たない 'data' オブジェクトになりました。
```

変数代入を連結しないでください。
変数代入を連鎖させると、暗黙のグローバル変数が作成されます。

```js
// bad
(function example() {
  // JavaScript はこれを次のように解釈します
  // let a = ( b = ( c = 1 ) );
  // let キーワードは変数 a にのみ適用されます。変数 b と c はグローバル変数になります。
  let a = b = c = 1;
}());

console.log(a); // throws ReferenceError
console.log(b); // 1
console.log(c); // 1

// good
(function example() {
  let a = 1;
  let b = a;
  let c = a;
}());

console.log(a); // throws ReferenceError
console.log(b); // throws ReferenceError
console.log(c); // throws ReferenceError

// the same applies for `const`
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
