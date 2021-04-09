## 命名規則

正しい名前付けを心掛けることにより、コードが洗練されます。
正しい名前を付けるには、正しいクラス構造が必要だからです。
意味のある、わかりやすい、正しい名前付けを心がけてください。

ただし、既存のコードを修正する場合は、そのスタイルに従ってください。
他のスタイルを混入させることは、品質の劣化となります。

* 1文字の名前は避ける。名前から意図が読み取れるようにすること。
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
* オブジェクト、関数、インスタンスにはキャメルケース（小文字から始まる）を使用すること。
```js
// bad
const OBJEcttsssss = {};
const this_is_my_object = {};
function c() {}

// good
const thisIsMyObject = {};
function thisIsMyFunction() {}
```
* クラスやコンストラクタにはパスカルケース（大文字から始まる）を使用すること。
```js
// bad
function user(options) {
  this.name = options.name;
}

const bad = new user({
  name: 'nope',
});

// good
class User {
  constructor(options) {
    this.name = options.name;
  }
}

const good = new User({
  name: 'yup',
});
```
* 末尾または先頭のアンダースコアを避ける。JavaScriptにアクセス修飾子が存在しないためです。
```js
// bad
this.__firstName__ = 'Panda';
this.firstName_ = 'Panda';
this._firstName = 'Panda';

// good
this.firstName = 'Panda';
```
* `this`の参照を保存するのを避ける。アロー関数かFunction#bindを利用すること。
```js
// bad
function foo() {
  const self = this;
  return function () {
    console.log(self);
  };
}

// bad
function foo() {
  const that = this;
  return function () {
    console.log(that);
  };
}

// good
function foo() {
  return () => {
    console.log(this);
  };
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
import index from './inside_directory/index'; // 明示的にフォルダを挟んでいます
import insideDirectory from './insideDirectory/index'; // 明示的にフォルダを挟んでいます

// good
import CheckBox from './CheckBox'; // すべてパスカルケースで記述されています
import fortyTwo from './fortyTwo'; // すべてキャメルケースで記述されています
import insideDirectory from './insideDirectory'; // すべてキャメルケースで記述され、indexは暗黙のものとして扱っています
// ^ insideDirectory.jsとinsideDirectory/index.jsのどちらも扱えます
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
* 略語および頭字語は、常にすべて大文字、またはすべて小文字にする必要があります。
```js
// bad
import SmsContainer from './containers/SmsContainer';

// bad
const HttpRequests = [
  // ...
];

// good
import SMSContainer from './containers/SMSContainer';

// good
const HTTPRequests = [
  // ...
];

// also good
const httpRequests = [
  // ...
];

// best
import TextMessageContainer from './containers/TextMessageContainer';

// best
const requests = [
  // ...
];
```
* （1）エクスポートされ、（2）`const`であり、そして（3）プログラマが、ネストされたプロパティも含めて、変更しないと信頼できる場合に限り、定数を大文字にすることができます。
```js
// bad
const PRIVATE_VARIABLE = 'エクスポートされていないものを不必要に大文字にしないでください';

// bad
export const THING_TO_BE_CHANGED = '定数が大文字ではありません';

// bad
export let REASSIGNABLE_VARIABLE = '変数に大文字を使用しないでください';

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