## jQuery

* jQueryオブジェクトの変数は、先頭に`$`を付与すること。
```js
// bad
const sidebar = $('.sidebar');

// good
const $sidebar = $('.sidebar');

// good
const $sidebarBtn = $('.sidebar-btn');
```
* jQueryの検索結果をキャッシュすること。
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
* DOMの検索には、`$('.sidebar ul')`や`$('.sidebar > ul')`のカスケードを使用すること。
* jQueryオブジェクトの検索には、スコープ付きの`find`を使用すること。
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