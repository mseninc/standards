## 分割代入

* 複数のプロパティからなるオブジェクトにアクセスする際は、オブジェクト構造化代入を使用すること。構造化代入を利用することで、プロパティの一時的な参照を減らすことができます。具体的には、
```js
function getFullName(user) {
  const firstName = user.firstName;
  const lastName = user.lastName;

  return `${firstName} ${lastName}`;
}
```
とするのではなく、
```js
function getFullName({ firstName, lastName }) {
  return `${firstName} ${lastName}`;
}
```
という風にして下さい。
* 配列の構造化代入を使用して下さい。
```js
const arr = [1, 2, 3, 4];

// bad
const first = arr[0];
const second = arr[1];

// good
const [first, second] = arr;
```
* 複数の値を返却する場合は、配列の構造化代入ではなく、オブジェクトの構造化代入を使用すること。呼び出し元に影響なく、後で新しいプロパティを追加したり、順序を変更することができます。
```js
// bad
function processInput(input) {
  return [left, right, top, bottom];
}

// 呼び出し元で返却されるデータの順番を考慮する必要があります。
const [left, __, top] = processInput(input);

// good
function processInput(input) {
  return { left, right, top, bottom };
}

// 呼び出し元は必要なデータのみ選択すればいい。
const { left, top } = processInput(input);
```