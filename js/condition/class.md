## クラスとコンストラクター

* `prototype`を直接操作することを避け、常に`class`を使用すること。
* 継承には`extends`を使用すること。
* メソッドの戻り値で`this`を返すことでメソッドチェーンを助けること。
```js
// bad
Jedi.prototype.jump = function () {
  this.jumping = true;
  return true;
};

Jedi.prototype.setHeight = function (height) {
  this.height = height;
};

const luke = new Jedi();
luke.jump(); // => true
luke.setHeight(20); // => undefined

// good
class Jedi {
  jump() {
    this.jumping = true;
    return this;
  }

  setHeight(height) {
    this.height = height;
    return this;
  }
}

const luke = new Jedi();

luke.jump()
  .setHeight(20);
```
* 独自の`toString()`の作成は認めますが、正しく動作することと副作用がないことを確認すること。
* 一つも指定されていない場合、クラスにはデフォルトのコンストラクタがあります。空のコンストラクタ関数やただ親クラスに移譲するだけのものは不要です。
* クラスのメンバの重複を避ける。重複したクラスメンバの宣言は暗黙的に最後のものが適用されます。重複を持つことはほぼ確実にバグです。