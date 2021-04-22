## ブロック

複数行のブロックには中括弧（ `{}` ）を使用して下さい。

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

`if` と `else` を使った複数行のブロックの場合は、 `else` ブロックの閉じ括弧と同じ行に `else` を置いてください。

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

`if` ブロックが `return` 文を必ず実行するのであれば、 `else` ブロックは不要です。

`if` ブロックが必ずしも `return` 文を実行しないのであれば、 `else` ブロックの中に `return` 文を書いても構いません。

```js
// bad
function foo() {
  if (x) {
    return x;
  } else {
    return y;
  }
}

// bad
function cats() {
  if (x) {
    return x;
  } else if (y) {
    return y;
  }
}

// bad
function dogs() {
  if (x) {
    return x;
  } else {
    if (y) {
      return y;
    }
  }
}

// good
function foo() {
  if (x) {
    return x;
  }

  return y;
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

// good
function dogs(x) {
  if (x) {
    if (z) {
      return y;
    }
  } else {
    return z;
  }
}
```