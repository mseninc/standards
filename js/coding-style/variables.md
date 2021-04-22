## 変数

変数を宣言する際は、常に `const` か `let` を使用して下さい。使用しない場合はグローバル変数として宣言されてしまいます。

```js
// bad
superPower = new SuperPower();

// good
const superPower = new SuperPower();
```

新しい変数を簡単に追加できるようにするために、1つの変数宣言に対して1つの `const` を利用して下さい。

```js
// bad
const items = getItems(),
    goSportsTeam = true,
    dragonball = 'z';

// bad
const items = getItems(),
    goSportsTeam = true;
    dragonball = 'z';

// good
const items = getItems();
const goSportsTeam = true;
const dragonball = 'z';
```

違う変数宣言を混在させるときは `const` 、 `let` の順に宣言してください。

```js
// bad
let i, len, dragonball,
    items = getItems(),
    goSportsTeam = true;

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

変数を割り当てる際は、必要かつ合理的な場所で割り当ててください。

```js
// bad - unnecessary function call
function checkName(hasName) {
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

  const name = getName();

  if (name === 'test') {
    this.setName('');
    return false;
  }

  return name;
}
```

変数代入を連結しないで下さい。変数代入を連鎖させると、暗黙のグローバル変数が作成されます。

```js
// bad
(function example() {
  // JavaScriptはこれを次のように解釈します
  // let a = ( b = ( c = 1 ) );
  // letキーワードは変数aにのみ適用されます。変数bとcはグローバル変数になります。
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

インクリメント演算子(`++`)デクリメント演算子(`--`)を使わないで下さい。
インクリメントおよびデクリメント演算子は自動セミコロン挿入の対象となり、アプリケーション内で値の増加または減少を伴う予期せぬエラーを引き起こす可能性があります。
`num++` や `num++` の代わりに `num += 1` のような構文で値を変更する方が様々な表現が可能です。
インクリメントおよびデクリメント演算子を使わないことで、直前の意図しない値の増加または減少による、プログラムでの予期しない動作を引き起こす可能性を防ぐことができます。

```js
// bad

const array = [1, 2, 3];
let num = 1;
num++;
--num;

let sum = 0;
let truthyCount = 0;
for (let i = 0; i < array.length; i++) {
  let value = array[i];
  sum += value;
  if (value) {
    truthyCount++;
  }
}

// good

const array = [1, 2, 3];
let num = 1;
num += 1;
num -= 1;

const sum = array.reduce((a, b) => a + b, 0);
const truthyCount = array.filter(Boolean).length;
```
* 代入で `=` の前後に改行を入れないで下さい。代入が `max-len` を超える場合は、値を丸括弧()で囲むこと。
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

未使用の変数を記述しないで下さい。未使用変数はコード内のスペースを占有し、他の開発者にとって混乱を招く可能性があります。

```js
// bad

var some_unused_var = 42;

// 書き込み専用変数は、使用していないものと見なされます。
var y = 10;
y = 5;

// これは、使用済みとは見なされません。
var z = 0;
z = z + 1;

// 関数の引数の一部が未使用になっています。
function getX(x, y) {
    return x;
}

// good

function getXPlusY(x, y) {
  return x + y;
}

var x = 1;
var y = a + 2;

alert(getXPlusY(x, y));

// 'type'はrestプロパティを持つため、未使用でも無視されます。
// これは、指定されたキーを省略したオブジェクトを抽出する形式です。
var { type, ...coords } = data;
// 'coords'は、'type'プロパティを持たない'data'オブジェクトになりました。
```