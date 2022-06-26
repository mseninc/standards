## プロパティ

プロパティにアクセスする場合は可能な限りドット (`.`) を使用してください。
ただし、プロパティ名に変数名として利用可能でない文字が含まれる場合はこの限りではありません。

```js
const luke = {
  jedi: true,
  age: 28,
  'i-know-him': false
};

// bad
const isJedi = luke['jedi'];

// good
const isJedi = luke.jedi;
const iKnowHim = luke['i-know-him'];
```

変数を使用してプロパティにアクセスする場合は角括弧 (`[]`) を使用してください。

```js
const luke = {
  jedi: true,
  age: 28,
};

function getProp(prop) {
  return luke[prop];
}

const isJedi = getProp('jedi');
```
