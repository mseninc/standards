## 変数

* 変数を宣言する際は、常に`const`か`let`を使用すること。使用しない場合はグローバル変数として宣言されてしまいます。グローバルな名前空間を汚染しないようにしましょう。
```js // bad
superPower = new SuperPower();

// good
const superPower = new SuperPower();
```
* 1つの変数宣言に対して1つの`const`を利用すること。この方法の場合、簡単に新しい変数を追加することができます。また、二度と区切り文字の違いによる`;`を`,`に置き換える作業を心配することがありません。すべての宣言を一度にジャンプするのではなく、デバッガーを使用して各宣言をステップスルーすることもできます。
```js
// bad
const items = getItems(),
    goSportsTeam = true,
    dragonball = 'z';

// bad
// (compare to above, and try to spot the mistake)
const items = getItems(),
    goSportsTeam = true;
    dragonball = 'z';

// good
const items = getItems();
const goSportsTeam = true;
const dragonball = 'z';
```
* まず`const`をグループ化し、その後`let`をグループ化すること。以前に割り当てられた変数に後で変数を割り当てる必要がある場合に役立ちます。
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
* 変数を割り当てる際は、必要かつ合理的な場所で行うこと。`let`と`const`はブロックスコープだからです。関数スコープではありません。
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
* 変数代入を連結しない。変数代入を連鎖させると、暗黙のグローバル変数が作成されます。
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
* インクリメント演算子(`++`)デクリメント演算子(`--`)を使わない。インクリメントおよびデクリメント演算子は自動セミコロン挿入の対象となり、アプリケーション内で値の増加または減少を伴う予期せぬエラーを引き起こす可能性があります。`num++`や`num++`の代わりに`num += 1`のような構文で値を変更する方が表現力豊かです。インクリメントおよびデクリメント演算子を許可しないことで、直前の意図しない値の増加または減少による、プログラムでの予期しない動作を引き起こす可能性を防ぐことができます。
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
* 代入で`=`の前後に改行を入れない。あなたの代入が`max-len`に違反している場合は、値を丸括弧()で囲むこと。
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
* 未使用の変数を記述しない。宣言されていてコードのどこにも使用されていない変数は、不完全なリファクタリングによる失敗である可能性が最も高いです。このような変数はコード内のスペースを占有し、他の開発者にとって混乱を招く可能性があります。
```js
// bad

var some_unused_var = 42;

// 書き込み専用変数は、使用していないものと見なされます。
var y = 10;
y = 5;

// それ自体の変更のための読み取りは、使用済みとは見なされません。
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