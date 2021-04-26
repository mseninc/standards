## jQuery

jQueryオブジェクトの変数は、先頭に `$` を付与して下さい。

```js
// bad
const sidebar = $('.sidebar');

// good
const $sidebar = $('.sidebar');

// good
const $sidebarBtn = $('.sidebar-btn');
```

jQueryの検索結果をキャッシュして下さい。

```js
// bad
function setSidebar() {
  $('.sidebar').hide();

  // ...stuff...

  $('.sidebar').css({
    'background-color': 'pink'
  });
}

// good
function setSidebar() {
  const $sidebar = $('.sidebar');
  $sidebar.hide();

  // ...stuff...

  $sidebar.css({
    'background-color': 'pink'
  });
}
```

DOMの検索には、 `$('.sidebar ul')`や`$('.sidebar > ul')` のように CSS セレクタを使用して下さい。

jQueryオブジェクトの検索には、スコープ付きの `find()` を使用して下さい。

```js
// bad
$('ul', '.sidebar').hide();

// bad
$('.sidebar').find('ul').hide();

// good
$('.sidebar ul').hide();

// good
$('.sidebar > ul').hide();

// good
$sidebar.find('ul').hide();
```