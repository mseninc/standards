## イベント

* データペイロードをイベント（DOMイベントまたはBackboneイベントのようなもっと独自のものかどうかにかかわらず）に添付するときは、生の値の代わりにオブジェクトリテラル（「ハッシュ」とも呼ばれる）を渡すこと。これにより、後の開発者は、イベントのすべてのハンドラを見つけて更新することなく、イベントペイロードにさらにデータを追加できます。
たとえば、下の例より、
```js
// bad
$(this).trigger('listingUpdated', listing.id);

...

$(this).on('listingUpdated', function(e, listingId) {
  // do something with listingId
});
```
こちらの方が好まれます:
```js
// good
$(this).trigger('listingUpdated', { listingId: listing.id });

...

$(this).on('listingUpdated', function(e, data) {
  // do something with data.listingId
});
```