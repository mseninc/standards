# MSEN JavaScript Coding Standard
JavaScriptのコーディング規約を定めます。
本規約は[Airbnb](https://mitsuruog.github.io/javascript-style-guide/#arrow-functions)のスタイルガイドを参考にしています。

## 変数の操作
### 型
1. 基本型
以下の型の変数は直接値を操作する。
- `string`
- `number`
- `boolean`
- `null`
- `undefined`
- `symbol`

例.
```JavaScript
let name = "Tari"
name = "Taro"
```

2. 参照型
以下の型の変数は参照を介して値を操作する。
- `object`
- `array`
- `function`

例.
```JavaScript
const arr = [ 'M', 'S', 'E', 'N' ]
arr[2] = 'O'

const bob = { gender: 'male', age: 17 }
bob.age = 35
```
