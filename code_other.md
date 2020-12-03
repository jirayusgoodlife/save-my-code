## Check select is has data select2 ? if have destroy before create new thing
```js
// remove initial value select 2
if ($('select').data('select2')) {
   $('select').select2('destroy');
}
// create initial value select2 again
$(".select2").select2();
```
