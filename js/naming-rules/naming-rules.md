## 命名規則

### 概要

変数名にはそのデータを表す名前を、関数名にはその機能を表す名前をつけるように心がけましょう。
また、変数名や関数名には **キャメルケース** を、クラス名には **パスカルケース** といった、命名規則を統一することも心がけましょう。

ただし、既存のコードを修正する場合は、そのスタイルに従ってください。
他のスタイルを混入させることは、品質の劣化となります。

* 1 文字の名前は避け、意味が明確にわかる名前をつけましょう。

```js
// bad
function l() {
  // ...
}

// good
function load() {
  // ...
}
```

* 変数名、関数名にはキャメルケース（小文字から始まる）を使用しましょう。

```js
// bad
const ThisIsMyOBJECT = {};
function this_is_my_function() {};

// good
const thisIsMyObject = {};
function thisIsMyFunction() {}
```

* クラス名にはパスカルケース（大文字から始まる）を使用しましょう。

```js
// bad
class user {
  constructor(options) {
    this.name = options.name;
  }
}

// good
class User {
  constructor(options) {
    this.name = options.name;
  }
}
```

* 末尾または先頭のアンダースコアを避けましょう。 JavaScript にアクセス修飾子が存在しないためです。

```js
// bad
this.__firstName__ = 'Panda';
this.firstName_ = 'Panda';
this._firstName = 'Panda';

// good
this.firstName = 'Panda';
```

* `this` の参照を保存するのを避けましょう。アロー関数か Function#bind を利用すること。

```js
// bad
function foo() {
  const self = this;
  return function () {
    console.log(self);
  };
}

// good
function foo() {
  return () => {
    console.log(this);
  };
}

//good
function foo(){
  return (function() {
    console.log(this);
  }).bind(this);
}
```

* ファイルを1つのクラスとしてエクスポートする場合、ファイル名はクラス名と完全に一致させること。

```js
// file 1 contents
class CheckBox {
  // ...
}
export default CheckBox;

// file 2 contents
export default function fortyTwo() { return 42; }

// file 3 contents
export default function insideDirectory() {}

// in some other file
// bad
import CheckBox from './checkBox'; // CheckBoxはパスカルケースでインポートされていますが、ファイル名はキャメルケースです
import FortyTwo from './FortyTwo'; // fortyTwoはキャメルケースでエクスポートされていますが、パスカルケースでインポートされています
import InsideDirectory from './InsideDirectory'; // fortyTwoと同様です

// bad
import CheckBox from './check_box'; // ファイル名がスネークケースです
import forty_two from './forty_two'; // キャメルケースでエクスポートされているものをスネークケースでインポートしています
import inside_directory from './inside_directory'; // fortyTwoと同様です

// good
import CheckBox from './CheckBox'; // すべてパスカルケースで記述されています
import fortyTwo from './fortyTwo'; // すべてキャメルケースで記述されています
```

* 関数をデフォルトエクスポートする場合、キャメルケースを使用すること。ファイル名は関数名と同じにすること。

```js
function makeStyleGuide() {
}
export default makeStyleGuide;
```

* コンストラクター / クラス / シングルトン / 関数ライブラリ をエクスポートする場合、パスカルケースを使用すること。

```js
const AirbnbStyleGuide = {
  es6: {
  }
};

export default AirbnbStyleGuide;
```

* オブジェクトをエクスポートする場合もパスカルケースを使用すること。

```js
const freeObject = {
  a = 0;
  b = 1;
  c = 2;
};

export default freeObject;
```
* （1）エクスポートされ、（2）`const`であり、そして（3）ネストされたプロパティも含めて、プログラマが変更しないと信頼できる場合に限り、定数を大文字にすることができます。

```js
// bad
const PRIVATE_VARIABLE = 'should not be unnecessarily uppercased within a file'; // エクスポートされていないものを不必要に大文字にしないでください

// bad
export const THING_TO_BE_CHANGED = 'should obviously not be uppercased'; // 定数が大文字ではありません

// bad
export let REASSIGNABLE_VARIABLE = 'do not use let with uppercase variables'; // 変数に大文字を使用しないでください

// ---

// 許可される例ですが、意味がありません
export const apiKey = 'SOMEKEY';

// better in most cases
export const API_KEY = 'SOMEKEY';

// ---

// bad - 意味がないのにKEYを大文字にしています
export const MAPPING = {
  KEY: 'value'
};

// good
export const MAPPING = {
  key: 'value'
};
```
