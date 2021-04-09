## ブロック

* 複数行のブロックには中括弧（`{}`）を使用すること。
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
* `if`と`else`を使った複数行のブロックの場合は、`else`ブロックの閉じ括弧と同じ行に`else`を置くこと。
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
* `if`ブロックが常にreturn文を実行するのであれば、それに続く`else`ブロックは不要です。`return`を含む`if`ブロックに続く `else if`ブロック内の`return`は複数の`if`ブロックに分けることができます。
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