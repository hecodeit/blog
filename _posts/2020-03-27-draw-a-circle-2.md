---
layout: post
title:  "Draw A Circle, Part 2"
date:   2020-03-27 14:27
---
### #11: Hatch
>The [arcsin](https://www.mathopenref.com/arcsin.html) function is the inverse of the sine function.
It returns the angle whose sine is a given number.

Draw lines inside circle with orientation and spacing. The arcsin(opposite/hypotenuse) can get the angel from circle center to the point with increasing space.
<canvas id="circle11_2"></canvas>

```javascript
function circle11(r) {
  var orientation = Math.random() * Math.PI * 2;
  var spacing = 20;
  for (var dist = 0; dist < r; dist += spacing) {
    var theta = Math.asin(dist / r) + orientation;
    var x1 = r * Math.cos(theta);
    var y1 = r * Math.sin(theta);
    var theta2 = Math.asin(dist / r) - orientation;
    var x2 = r * Math.cos(theta2 - Math.PI);
    var y2 = r * Math.sin(theta2);
    var theta3 = Math.asin(dist / r) + orientation + Math.PI;
    var x3 = r * Math.cos(theta3);
    var y3 = r * Math.sin(theta3);
    var theta4 = Math.asin(dist / r) - orientation + Math.PI;
    var x4 = r * Math.cos(theta4 - Math.PI);
    var y4 = r * Math.sin(theta4);
    context.beginPath();
    context.moveTo(x1, y1);
    context.lineTo(x2, y2);
    context.stroke();

    if (dist > 0) {
      context.beginPath();
      context.moveTo(x3, y3);
      context.lineTo(x4, y4);
      context.stroke();
    }
  }
}
```
<canvas id="circle11"></canvas>

### #12: Lines from center to border with some noise
Similar with #2, but the line segment with parts, and move with noise.
```javascript
function circle12(r) {
  for(var a=0; a<Math.PI*2; a+=Math.PI*2/180+Math.random()*.08-0.04){
    context.beginPath();
    for (var j = 0; j < r; j += r/20) {
      var nx = j*Math.cos(a)+ Math.random()*20-10;
      var ny = j*Math.sin(a)+ Math.random()*20-10;
      context.lineTo(nx, ny);
    }
    context.stroke();
  }
}
```
<canvas id="circle12"></canvas>

<script>
var width, height, pixelRatio;
var functionNum = 1;

function map_range(value, low1, high1, low2, high2) {
  return low2 + (high2 - low2) * (value - low1) / (high1 - low1);
}

function stop() {
  isRun = !isRun;
}

function counting() {
  counter++;
}

function circle11(x, y, r, context) {
  var rt = Math.random() * Math.PI * 2;

  var step = 20;

  for (var dist = 0; dist < r; dist += step) {
    var theta = Math.asin(dist / r) + rt;

    var x1 = r * Math.cos(theta);
    var y1 = r * Math.sin(theta);

    var theta2 = Math.asin(dist / r) - rt;
    var x2 = r * Math.cos(theta2 - Math.PI);
    var y2 = r * Math.sin(theta2);

    var theta3 = Math.asin(dist / r) + rt + Math.PI;
    var x3 = r * Math.cos(theta3);
    var y3 = r * Math.sin(theta3);
    var theta4 = Math.asin(dist / r) - rt + Math.PI;
    var x4 = r * Math.cos(theta4 - Math.PI);
    var y4 = r * Math.sin(theta4);

    context.beginPath();
    context.moveTo(x1, y1);
    context.lineTo(x2, y2);
    context.stroke();

    if (dist > 0) {
      context.beginPath();
      context.moveTo(x3, y3);
      context.lineTo(x4, y4);
      context.stroke();
    }
  }

}

function circle11_2(x, y, r, context) {
  var rt = Math.PI * 2;
  var dist = 50;
  for(var i=1; i<r/dist; i++){
    var theta = Math.asin(dist*i / r) + rt;
    var x1 = r * Math.cos(theta);
    var y1 = r * Math.sin(theta);

    var theta2 = Math.asin(dist*i / r) - rt;
    var x2 = r * Math.cos(theta2 - Math.PI);
    var y2 = r * Math.sin(theta2);

    context.beginPath();
    context.moveTo(x1+x, y1+y);
    context.lineTo(x2+x, y2+y);
    context.lineTo(x, y);
    context.closePath();
    context.stroke();
  }

  context.beginPath();
  context.arc(x, y, r-2, 0, 2 * Math.PI);
  context.stroke();
}

function circle12(x, y, r, context) {
  for(var a=0; a<Math.PI*2; a+=Math.PI*2/180+Math.random()*.08-0.04){
    context.beginPath();
    for (var j = 0; j < r; j += r/20) {
      var nx = x+j*Math.cos(a)+ Math.random()*20-10;
      var ny = y+j*Math.sin(a)+ Math.random()*20-10;
      context.lineTo(nx, ny);
    }
    context.stroke();
  }
}


var bodyWidth;
function resize(canvas) {
  pixelRatio = window.devicePixelRatio;
  bodyWidth = window.innerWidth-40;
  if(bodyWidth >500){
    bodyWidth = 500;
  }
  width = bodyWidth;
  height = bodyWidth+30;
  canvas.width = width * pixelRatio;
  canvas.height = height * pixelRatio;
  canvas.style.width = "".concat(width, "px");
  canvas.style.height = "".concat(height, "px");
}

function draw(context, index) {

  context.strokeStyle = 'rgba(0,0,0,0.2)';

  context.save();
  context.translate(width * pixelRatio/ 2, height * pixelRatio / 2);

  eval('circle' + index + '(0,-30,bodyWidth,context)');
  context.font = "32px sans-serif";
  context.fillText('#' + index, 0, bodyWidth+20);
  context.restore();
}

function init() {

  for(var i=11; i<13; i++){
    const canvas = document.getElementById('circle'+i);
    const context = canvas.getContext('2d');

    resize(canvas);
    context.lineWidth = pixelRatio;
    context.lineJoin = 'round';
    context.textAlign = 'center';
    context.fillStyle = 'rgba(0,0,0,0.2)';

    draw(context, i);
  }
}

init();

function drawAdditional(){
  const canvas = document.getElementById('circle11_2');
  const context = canvas.getContext('2d');

  resize(canvas);
  context.lineWidth = pixelRatio;
  context.lineJoin = 'round';
  context.textAlign = 'center';

  context.strokeStyle = 'rgba(0,0,0,0.2)';
  context.fillStyle = 'rgba(0,0,0,0.2)';

  context.save();
  context.translate(width * pixelRatio/ 2, height * pixelRatio / 2);
  circle11_2(0,-30,bodyWidth,context);
  context.font = "32px sans-serif";
  context.fillText('arcsin(opposite/hypotenuse)', 0, bodyWidth+20);
  context.restore();
}
drawAdditional();

console.log("ciao")
</script>
