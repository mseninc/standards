## ブロック

複数行のブロックには中括弧 (`{}`) を使用して下さい。

```js
// bad
if (test)
  return false;

// good
if (test) return false;

// good
if (test) {
  return false;
}

// bad
function foo() { return false; }

// good
function bar() {
  return false;
}
```

`if` と `else` を使った複数行のブロックの場合は、 `if` ブロックの閉じ括弧と同じ行に `else` を置いてください。

```js
// bad
if (test) {
  thing1();
  thing2();
}
else {
  thing3();
}

// good
if (test) {
  thing1();
  thing2();
} else {
  thing3();
}
```

`if` ブロックに `return` 文が存在する場合、可能な限り `else` ブロックに `return` 文は書かずに外側のブロックで `return` してください。

```js
// bad
function foo() {
  if (x) {
    return x;
  } else {
    return y;
  }
}

// good
function foo() {
  if (x) {
    return x;
  }

  return y;
}
```

`if` ブロックと `else if` ブロックでそれぞれ `return` 文が含まれる場合、 `else if` ブロックを別の `if` ブロックに分けてください。

```js
// bad
function cats() {
  if (x) {
    return x;
  } else if (y) {
    return y;
  }
}

// good
function cats() {
  if (x) {
    return x;
  }

  if (y) {
    return y;
  }
}
```
