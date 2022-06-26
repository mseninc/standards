## イベント

イベントハンドラにデータを渡す場合、オブジェクトとして渡してください。
イベントハンドラに新たにデータを渡す場合は、オブジェクトのプロパティを追加するようにしてください。

```js
// bad
$(this).trigger('listingUpdated', listing.id);
...
$(this).on('listingUpdated', function(e, listingId) {
  // do something with listingId
});

// good
$(this).trigger('listingUpdated', { listingId: listing.id });
...
$(this).on('listingUpdated', function(e, data) {
  // do something with data.listingId
});
```