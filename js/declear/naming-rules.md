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
* 末尾または先頭のアンダースコアを避ける。 先頭のアンダースコアは「非公開」を意味する一般的な規則ですが、JavaScriptは、プロパティやメソッドを隠す事ができません。
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
import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export

// bad
import CheckBox from './check_box'; // PascalCase import/export, snake_case filename
import forty_two from './forty_two'; // snake_case import/filename, camelCase export
import inside_directory from './inside_directory'; // snake_case import, camelCase export
import index from './inside_directory/index'; // requiring the index file explicitly
import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly

// good
import CheckBox from './CheckBox'; // PascalCase export/import/filename
import fortyTwo from './fortyTwo'; // camelCase export/import/filename
import insideDirectory from './insideDirectory'; // camelCase export/import/directory name/implicit "index"
// ^ supports both insideDirectory.js and insideDirectory/index.js
```
* 関数をデフォルトエクスポートする場合、キャメルケースを使用すること。ファイル名は関数名と同じにすること。
```js
function makeStyleGuide() {
}
export default makeStyleGuide;
```
* コンストラクター / クラス / シングルトン / 関数ライブラリ / ベアオブジェクトをエクスポートする場合、パスカルケースを使用すること。
```js
const AirbnbStyleGuide = {
  es6: {
  }
};

export default AirbnbStyleGuide;
```
* 略語およびinitialisms（複数の単語で構成される言葉の頭文字をとって一つの単語にしたもの）は、常にすべて大文字、またはすべて小文字にする必要があります。
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
* （1）エクスポートされ、（2）`const`（再割り当てできない）であり、そして（3）プログラマがそれを（そしてネストされたプロパティも）変更しないと信頼できる場合に限り、定数を大文字にすることができます。
```js
// bad
const PRIVATE_VARIABLE = 'should not be unnecessarily uppercased within a file';

// bad
export const THING_TO_BE_CHANGED = 'should obviously not be uppercased';

// bad
export let REASSIGNABLE_VARIABLE = 'do not use let with uppercase variables';

// ---

// allowed but does not supply semantic value
export const apiKey = 'SOMEKEY';

// better in most cases
export const API_KEY = 'SOMEKEY';

// ---

// bad - unnecessarily uppercases key while adding no semantic value
export const MAPPING = {
  KEY: 'value'
};

// good
export const MAPPING = {
  key: 'value'
};
```