# bootstrap-3.3.7
Lastest and last version of Bootstrap 3, but with a fix for the `Uncaught TypeError: Cannot read property 'off' of null` error, when using Tooltips.

![alt text](https://cloud.githubusercontent.com/assets/9087223/22259228/549e32ce-e219-11e6-80b9-d98961809ba1.png)

See issue here, https://github.com/twbs/bootstrap/issues/21830

### The flowing was changed in the original file.
At line 1489
```
var complete = function () {
  var prevHoverState = that.hoverState
  that.$element.trigger('shown.bs.' + that.type)
  that.hoverState = null

  if (prevHoverState == 'out') that.leave(that)
}
```
to
```
var complete = function () {
  var prevHoverState = that.hoverState
  null != that.$element && that.$element.trigger('shown.bs.' + that.type)
  that.hoverState = null

  if (prevHoverState == 'out') that.leave(that)
}
```
And line 1731
```
Tooltip.prototype.destroy = function () {
  var that = this
  clearTimeout(this.timeout)
  this.hide(function () {
    that.$element.off('.' + that.type).removeData('bs.' + that.type)
    if (that.$tip) {
      that.$tip.detach()
    }
    that.$tip = null
    that.$arrow = null
    that.$viewport = null
    that.$element = null
  })
}
```
to 
```
Tooltip.prototype.destroy = function () {
  var that = this
  clearTimeout(this.timeout)
  this.hide(function () {
    null != that.$element && that.$element.off('.' + that.type).removeData('bs.' + that.type)
    if (that.$tip) {
      that.$tip.detach()
    }
    that.$tip = null
    that.$arrow = null
    that.$viewport = null
    that.$element = null
  })
}
```
