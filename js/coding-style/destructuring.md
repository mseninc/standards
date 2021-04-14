## 分割代入

配列の要素やオブジェクトのプロパティを変数に代入する場合は、分割代入を使用してください。。

```js
const arr = [1, 2, 3, 4];
const obj = { ward: 'Fukushima', city: 'Osaka', pref: 'Osaka' };

// bad
const first = arr[0];
const second = arr[1];
const ward = obj.ward;
const city = obj.city;

// good
const [first, second] = arr;
const { ward, city } = obj;
```

関数の引数に与えられたオブジェクトのプロパティにアクセスする場合、関数の引数に対してオブジェクトの分割代入を使用してください。。

```js
// bad
function getFullName(user) {
  const firstName = user.firstName;
  const lastName = user.lastName;

  return `${firstName} ${lastName}`;
}

// good
function getFullName({ firstName, lastName }) {
  return `${firstName} ${lastName}`;
}
```
