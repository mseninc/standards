## 分割代入

* 複数のプロパティからなるオブジェクトにアクセスする際は、オブジェクト構造化代入を使用すること。構造化代入を利用することで、それらのプロパティのための中間的な参照を減らすことができます。具体的には、
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
* 複数の値を返却する場合は、配列の構造化代入ではなく、オブジェクトの構造化代入を使用すること。こうすることで、後で新しいプロパティを追加したり、呼び出し元に影響することなく順序を変更することができます。