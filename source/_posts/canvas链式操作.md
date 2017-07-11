---
title: canvas链式操作
date: 2017-07-11 14:45:04
tags:
  - canvas
---

## 链式操作

```javascript
function Canvas2DContext(canvas) {
  if (typeof canvas === 'string') {
    canvas = document.getElementById(canvas);
  }
  if (!(this instanceof Canvas2DContext)) {
    return new Canvas2DContext(canvas);
  }
  this.context = this.ctx = canvas.getContext('2d');
  if (!Canvas2DContext.prototype.arc) {
    Canvas2DContext.setup.call(this, this.ctx);
  }
}
Canvas2DContext.setup = function() {
  var methods = ['arc', 'arcTo', 'beginPath', 'bezierCurveTo', 'clearRect', 'clip',
    'closePath', 'drawImage', 'fill', 'fillRect', 'fillText', 'lineTo', 'moveTo',
    'quadraticCurveTo', 'rect', 'restore', 'rotate', 'save', 'scale', 'setTransform',
    'stroke', 'strokeRect', 'strokeText', 'transform', 'translate'];
  var getterMethods = ['createPattern', 'drawFocusRing', 'isPointInPath', 'measureText', // drawFocusRing not currently supported
    // The following might instead be wrapped to be able to chain their child objects
    'createImageData', 'createLinearGradient',
    'createRadialGradient', 'getImageData', 'putImageData'
  ];
  var props = ['canvas', 'fillStyle', 'font', 'globalAlpha', 'globalCompositeOperation',
    'lineCap', 'lineJoin', 'lineWidth', 'miterLimit', 'shadowOffsetX', 'shadowOffsetY',
    'shadowBlur', 'shadowColor', 'strokeStyle', 'textAlign', 'textBaseline'];
  for (let m of methods) {
    let method = m;
    Canvas2DContext.prototype[method] = function() {
      this.ctx[method].apply(this.ctx, arguments);
      return this;
    };
  }
  for (let m of getterMethods) {
    let method = m;
    Canvas2DContext.prototype[method] = function() {
      return this.ctx[method].apply(this.ctx, arguments);
    };
  }
  for (let p of props) {
    let prop = p;
    Canvas2DContext.prototype[prop] = function(value) {
      if (value === undefined)
        return this.ctx[prop];
      this.ctx[prop] = value;
      return this;
    };
  }
};
var canvas = document.getElementById('canvas');
// Use context to get access to underlying context
var ctx = Canvas2DContext(canvas)
  .strokeStyle('rgb(30, 110, 210)')
  .transform(10, 3, 4, 5, 1, 0)
  .strokeRect(2, 10, 15, 20)
  .context;
// Use property name as a function (but without arguments) to get the value
var strokeStyle = Canvas2DContext(canvas)
  .strokeStyle('rgb(50, 110, 210)')
  .strokeStyle();
```
