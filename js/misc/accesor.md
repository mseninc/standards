## アクセサ

JavaScript の ゲッター/セッターは使用せず、 `getVal()` や `setVal('hello')` などのアクセサ関数を作成してください。

```js 
// bad
class Dragon {
  get age() {
    // ...
  }

  set age(value) {
    // ...
  }
}

// good
class Dragon {
  getAge() {
    // ...
  }

  setAge(value) {
    // ...
  }
}
```

プロパティ/メソッドが `boolean` の場合は、 `isVal()` または `hasVal()` のように命名してください。

```js
// bad
if (!dragon.age()) {
  return false;
}

// good
if (!dragon.hasAge()) {
  return false;
}
```

`get()` や `set()` という関数を作成する場合、名前と処理に一貫性を持たせてください。

```js
class Jedi {
  constructor(options = {}) {
    const lightsaber = options.lightsaber || 'blue';
    this.set('lightsaber', lightsaber);
  }

  set(key, val) {
    this[key] = val;
  }

  get(key) {
    return this[key];
  }
}
```
