## イベント

* データをイベントに添付する場合、生の値ではなく、オブジェクトリテラルを渡します。
これにより、後の開発者は、イベントのハンドラをすべて見つけて更新することなく、イベントペイロードにデータを追加することができます。
たとえば、下の例より、
```js
// bad
$(this).trigger('listingUpdated', listing.id);

...

$(this).on('listingUpdated', function(e, listingId) {
  // do something with listingId
});
```
こちらの方が好まれます。
```js
// good
$(this).trigger('listingUpdated', { listingId: listing.id });

...

$(this).on('listingUpdated', function(e, data) {
  // do something with data.listingId
});
```