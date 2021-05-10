## 巻き上げ

`var` 宣言は、それらに最も近い包含関数スコープの一番上に引き上げられますが、代入された値は引き継がれません。

```js
// notDefined がグローバル変数に存在しない場合、うまく動作しません。
function example() {
  console.log(notDefined); // => throws a ReferenceError
}

// 特定の変数を参照するコードの後でその変数を宣言した場合、
// 変数が巻き上げられた上で動作します。
// 注意 :`true` という値自体は巻き上げられません。
function example() {
  console.log(declaredButNotAssigned); // => undefined
  var declaredButNotAssigned = true;
}

// インタープリンタは変数宣言をスコープの先頭に巻き上げます。
// 上の例は次のように書き直すことが出来ます。
function example() {
  let declaredButNotAssigned;
  console.log(declaredButNotAssigned); // => undefined
  declaredButNotAssigned = true;
}

// const と let を利用した場合
function example() {
  console.log(declaredButNotAssigned); // => throws a ReferenceError
  console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
  const declaredButNotAssigned = true;
}
```

無名関数の場合、関数が割当てされる前の変数が巻き上げられます。

```js
function example() {
  console.log(anonymous); // => undefined

  anonymous(); // => TypeError anonymous is not a function

  var anonymous = function() {
    console.log('anonymous function expression');
  };
}
```

名前付き関数の場合も同様に変数が巻き上げられます。関数名や関数本体は巻き上げられません。

```js
function example() {
  console.log(named); // => undefined

  named(); // => TypeError named is not a function

  superPower(); // => ReferenceError superPower is not defined

  var named = function superPower() {
    console.log('Flying');
  };
}

// 関数名と変数名が同じ場合も同じことが起きます。
function example() {
  console.log(named); // => undefined

  named(); // => TypeError named is not a function

  var named = function named() {
    console.log('named');
  }
}
```

関数宣言は関数名と関数本体が巻き上げられます。

```js
function example() {
  superPower(); // => Flying

  function superPower() {
    console.log('Flying');
  }
}
```