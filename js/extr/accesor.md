## アクセサ

* プロパティのためのアクセサ関数は必須ではありません。
* 予期しない副作用を引き起こし、テスト、保守、および理由の説明が難しいため、JavaScriptのゲッター/セッターは使用しないこと。代わりに、アクセサ関数を作るのであれば、`getVal()`とか`setVal('hello')`とすること。
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
* プロパティ/メソッドが`boolean`の場合は、`isVal()`または`hasVal()`を使用すること。
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
* `get()`や`set()`という関数を作成しても構いませんが、一貫性を持たせてください。
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