# データの操作
本章では、 データの操作に関するコーディング規約を制定します。

## 型
1. 基本型
以下の型の変数は直接値を操作する。
- `string`
- `number`
- `boolean`
- `null`
- `undefined`
- `symbol`

```JavaScript
let name = "Tari"
name = "Taro"
```

2. 参照型
以下の型の変数は参照を介して値を操作する。
- `object`
- `array`
- `function`

```JavaScript
const arr = ['M', 'S', 'E', 'N']
arr[2] = 'O'

const bob = {
  gender: 'male',
  age: 17
}
bob.age = 35
```

---

## 参照
参照型の変数を宣言する際には、 `const` 宣言子を使用する。
参照を再割当てする変数を宣言する際には、 `let` 宣言子を使用する。

```JavaScript
const hoge = 1
let piyo = 2
piyo += 10
```

---

## オブジェクト
1. オブジェクトを生成する際には、 リテラル構文を使用する。

```JavaScript
const obj = {}
```

2. 動的なプロパティ名を持つオブジェクトを生成する際には、 [computed property names](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Object_initializer#%E8%A8%88%E7%AE%97%E3%81%95%E3%82%8C%E3%81%9F%E3%83%97%E3%83%AD%E3%83%91%E3%83%86%E3%82%A3%E5%90%8D)を使用する。

```JavaScript
function getKey(id) {
  return `key-${id}`
}

const city = {
  zipCode: '553-0001',
  name: 'Ebie Fukushima Ward Osaka City',
  [getKey('MSEN')]: 'MSEN Inc.'
}
```

3. メソッドの定義には、メソッドの短縮構文を使用する。

```JavaScript
const cat = {
  name: 'Oliver',

  say() {
    return 'Mew, I am Oliver.'
  },
}
```

4. オブジェクトのプロパティを宣言する際には、プロパティの短縮構文を使用する。

```JavaScript
const cpu = 'Intel Core i7 7600U'
const os = 'FreeBSD 12.0-RELEASE'

const machine = {
  cpu,
  os,
}
```

5. プロパティの短縮構文は、オブジェクトの宣言の先頭にまとめる

```JavaScript
const cpu = 'Intel Core i7 7600U'
const os = 'FreeBSD 12.0-RELEASE'

const machine = {
  cpu,
  os,
  memory: 128,
  user: 'Oliver',
}
```

6. プロパティが識別子の命名規則 (`/^[_a-zA-Z$](\w|\$)*$/`) に則っていない場合にのみ、プロパティを引用符で括る。
```JavaScript
const bob = {
  name: 'Bob Crocker'
  age: 35,
  '553-0001': 'Ebie, Fukushima Ward, Osaka City',
}
```

7. 以下のような `object.prototype` のメソッドを直接呼び出さない。
  - `hasOwnProperty`
  - `propertyIsEnumerable`
  - `isPrototypeOf`

```JavaScript
// good
console.log(Object.prototype.hasOwnProperty.call(object, key))

// one of the best way
const has = Object.prototype.hasOwnProperty
// another way
import has from 'has' // https://www.npmjs.com/package/has

console.log(has.call(object, key))
```

8. オブジェクトの浅いコピー (shallow-copy) をする際には、[スプレッド構文](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Spread_syntax) を使用する。特定のプロパティを省略したオブジェクトを新しく取得する場合は、オブジェクトのレスト演算子を使用する。

```JavaScript
const original = {
  a: 1,
  b: 2
}
const copied = { ...original,
  c: 3
} // { a: 1, b: 2, c: 3 }

const {
  a,
  ...noA
} = copied // noA => { b: 2, c: 3 }
```

## 配列

1. 配列を生成する際には、 リテラル構文を使用する。 

```JavaScript
const items = []
```

2. 配列の末尾に要素を追加する際には、直接配列に代入せず、`push` メソッドを利用する。

``` JavaScript
const items = []
items.push('hogehoge')
```

3. 配列をコピーする場合、配列の拡張演算子 `...` を利用する。

```JavaScript
const itemsCopy = [...items]
```

4. イテレータブルなオブジェクトを配列に変換する際には、`from` メソッドの代わりに `...` 演算子を利用する。

```JavaScript
let node
const foo = document.querySelectorAll('.foo')

// good
node = Array.from(foo)

// best
node = [...foo]
```

5. 配列ライクなオブジェクトを配列に変換する場合は、 `from` メソッドを利用する。

```JavaScript
const arrLike = {
  0: 'hoge',
  1: 'piyo',
  2: 'foo',
  length: 3
}

const arr = Array.from(arrLike)
```

6. イテレータブルなオブジェクトへのマッピングには、 スプレッド構文の代わりに `from` メソッドを利用する。 

```JavaScript
const baz = Array.from(foo, bar)
```

7. 配列のコールバック関数中では、 `return` 文を使用する。ただし、関数内の文が１つで、かつ副作用のない式を返す場合は `return` を省略してよい。

```JavaScript
[1, 2, 3].map((x) => {
  const y = x + 1
  return x * y
})

[1, 2, 3].map(x => x + 1)
```

8. 配列の定義が複数行に渡る際は、配列の開き括弧の直後と閉じ括弧の直前で改行する。また、各要素ごとに改行を入れる。

```JavaScript
// 
const numArr = [
  1,
  2
]

const objArr = [{
    id: 1,
  },
  {
    id: 2,
  },
]
```
