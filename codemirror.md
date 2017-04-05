# To use code mirror as a readonly displaying area,

```javascript
$('.code').each(function() {

    var $this = $(this),
        $code = $this.html();

    $this.empty();

    var myCodeMirror = CodeMirror(this, {
        value: $code,
        mode: 'javascript',
        lineNumbers: !$this.is('.inline'),
        readOnly: true
    });

});
```
