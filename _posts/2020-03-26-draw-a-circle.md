---
layout: post
title:  "Draw A Circle, Part 1"
date:   2020-03-26 11:33
---

### Motivation
Inspired by Manohar Vanga's blog [Seventy-Five Ways to Draw a Circle](https://sighack.com/post/seventy-five-ways-to-draw-a-circle). I'm learning Manohar's code and try understand the math behind drawing circle.

I want the code can be use by pen plotter later, so don't use color, grayscale or line width of line. Only focus on how drawing circles by 1 pixel black line.

Also I don't use any javascript library, done by javascript with HTML5 canvas 2d API only.

### #1: Simple circle
>A circle can be defined as the locus of all points that satisfy the equations
x = r cos(t)    y = r sin(t)
where x, y are the coordinates of any point on the circle, r is the radius of the circle and
t is the parameter - the angle subtended by the point at the circle's center
>[Algorithm to draw circles and ellipses](https://www.mathopenref.com/coordcirclealgorithm.html)

```javascript
function circle1(r) {
  var numberDivided = 10;
  var step = 2*Math.PI/numberDivided;

  context.beginPath();
  for (var theta = 0; theta < Math.PI*2; theta += step) {
    var x = r*Math.cos(theta);
    var y = r*Math.sin(theta);
    context.lineTo(x, y);
  }
  context.closePath();
  context.stroke();
}
```
<canvas id="circle1"></canvas>

Defined the number of divided of a circle, you can find how much angel increasing per step. It's like you want divide a pie equally 10 part, so each pice of pie is 36 degree. Firstly, ```cos(0)``` and ```sin(0)``` is the begin x y position as ```x=1, y=0```. The next point is ```cos(36)``` and ```sin(36)```, the process loop 10 times.

It's can add more points or change the method of angel increasing using random or noise.

<canvas id="circle1_2"></canvas>
### #2: Random Lines from center to border
As #1 showing, using ```x = r*cos(t);y = r*sin(t);``` find point on circle, it's easy to draw lines from center to border.

```javascript
function circle2(r) {
  for (var a = Math.random(); a < 180; a += 1 + Math.random() * 0.1234) {
    context.beginPath();
    context.moveTo(x, y);
    var x = r*Math.cos(a);
    var y = r*Math.sin(a);
    context.lineTo(x, y);
    context.stroke();
  }
}
```
<canvas id="circle2"></canvas>

### #3: Spiral fill
Draw a spiral like draw circle, but reduce the radius by each step.
```javascript
function circle3(r) {
  context.beginPath();
  for (var a = 0; a < Math.PI * 40; a += Math.PI * 2 / 100) {
    var x = Math.cos(a) * r;
    var y = Math.sin(a) * r;
    context.lineTo(x, y);
    r -= .2;
  }
  context.stroke();
}
```
<canvas id="circle3"></canvas>
### #4: Cone fill
Start from a random point of circle, continue draw circles with increasing of radius.
```javascript
function circle4(r) {
  var startAngel = Math.random() * Math.PI * 2;

  for (var a = 0; a < 2 * r; a += 10) {
    context.beginPath();
    var x = Math.cos(startAngel) * (r - a / 2);
    var y = Math.sin(startAngel) * (r - a / 2);
    context.arc(x, y, a / 2, 0, 2 * Math.PI);
    context.stroke();
  }
}
```
<canvas id="circle4"></canvas>
### #5: Draw random circle from  board
Same idea with #4 Cone fill, only draw each circle with random angel.
```javascript
function circle5(r) {
  for (var a = 0; a < 2 * r; a += 10) {
    var startAngel = Math.random() * Math.PI * 2;
    context.beginPath();
    var x = Math.cos(startAngel) * (r - a / 2);
    var y = Math.sin(startAngel) * (r - a / 2);
    context.arc(x, y, a / 2, 0, 2 * Math.PI);
    context.stroke();
  }
}
```
<canvas id="circle5"></canvas>
### #6: Random line
Find two random point on circle, draw line. Repeat as much as needed.
```javascript
function circle6(r) {
  for (var i = 0; i < 40; i++) {
    var randomAngelA = Math.random() * Math.PI * 2;
    var randomAngelB = Math.random() * Math.PI * 2;
    var x1 = r * Math.cos(randomAngelA);
    var y1 = r * Math.sin(randomAngelA);
    var x2 = r * Math.cos(randomAngelB);
    var y2 = r * Math.sin(randomAngelB);
    context.beginPath();
    context.moveTo(x1, y1);
    context.lineTo(x2, y2);
    context.stroke();
  }
}
```
<canvas id="circle6"></canvas>
### #7: Fill inside circle
Not like #5 method, but find random point from center.
```javascript
function circle7(r) {
  for (var a = 0; a < 1000; a++) {
    var randomRadius = Math.random() * r;
    var randomAngle = Math.random() * (Math.PI * 2);
    context.beginPath();
    var x = Math.cos(randomAngle) * randomRadius;
    var y = Math.sin(randomAngle) * randomRadius;
    context.arc(x, y, 10, 0, 2 * Math.PI);
    context.stroke();
  }
}
```
<canvas id="circle7"></canvas>
### #8: Fill inside circle B
I find the #7 circle fill not perfect but the draw more circle near the center of circle. After read [How to generate a random point within a circle of radius R](https://stackoverflow.com/a/50746409), I get better result of random fill inside circle.
```javascript
function circle8(r) {
  for (var a = 0; a < 1000; a++) {
    var randomRadius = r * Math.sqrt(Math.random());
    var randomAngle = Math.random() * Math.PI * 2;
    context.beginPath();
    var x = Math.cos(randomAngle) * randomRadius;
    var y = Math.sin(randomAngle) * randomRadius;
    context.arc(x, y, 10, 0, 2 * Math.PI);
    context.stroke();
  }
}
```
<canvas id="circle8"></canvas>

### #9: Draw circles from center
Draw circle from center with very small size, continuously draw circle by increasing the radius.
```javascript
function circle9(r) {
  for (var i = 0.4; i <= r; i += 10) {
    context.beginPath();
    context.arc(0, 0, i, 0, 2 * Math.PI);
    context.stroke();
  }
}
```
<canvas id="circle9"></canvas>
### #10: Draw arcs from center
Same with #9, but draw arcs instead.
```javascript
function circle10(r) {
  for (var i = 0.2; i <= r; i += 10) {
    context.beginPath();
    context.arc(0, 0, i, Math.random() * Math.PI, Math.random() * Math.PI * 2);
    context.stroke();
  }
}
```
<canvas id="circle10"></canvas>


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


function circle1(x, y, r, context) {
  context.beginPath();
  for (var a = 0; a < Math.PI * 2; a += Math.PI * 2 / 10) {
    var xoff = Math.cos(a) * r + x;
    var yoff = Math.sin(a) * r + y;
    context.lineTo(xoff, yoff);
  }
  context.closePath();
  context.stroke();
}

function circle1_2(x, y, r, context) {
  for (var a = 0; a < Math.PI * 2; a += Math.PI * 2 / (Math.random() * 100+50)) {
    var xoff = Math.cos(a) * r + x;
    var yoff = Math.sin(a) * r + y;
    context.beginPath();
    context.arc(xoff, yoff, 2, 0, 2 * Math.PI);
    context.closePath();
    context.stroke();
  }

}

function circle2(x, y, r, context) {
  for (var a = Math.random(); a < 180; a += 1 + Math.random() * 0.1234) {
    context.beginPath();
    context.moveTo(x, y);
    var xoff = Math.cos(a) * r + x;
    var yoff = Math.sin(a) * r + y;
    context.lineTo(xoff, yoff);
    context.stroke();
  }
}

function circle3(x, y, r, context) {
  context.beginPath();
  for (var a = 0; a < Math.PI * 40; a += Math.PI * 2 / 100) {
    var xoff = Math.cos(a) * r + x;
    var yoff = Math.sin(a) * r + y;
    context.lineTo(xoff, yoff);
    r -= .2;
  }
  context.stroke();
}


function circle4(x, y, r, context) {
  var rt = Math.random() * Math.PI * 2;

  for (var a = 0.3; a < 2 * r; a += Math.random() * 30 + 1) {
    context.beginPath();
    var xoff = Math.cos(rt) * (r - a / 2) + x;
    var yoff = Math.sin(rt) * (r - a / 2) + y;
    context.arc(xoff, yoff, a / 2, 0, 2 * Math.PI);
    context.stroke();
  }
}


function circle5(x, y, r, context) {
  for (var a = 0; a < 2 * r; a += Math.random() * 30 + 1) {
    var rt = Math.random() * Math.PI * 2;
    context.beginPath();
    var xoff = Math.cos(rt) * (r - a / 2) + x;
    var yoff = Math.sin(rt) * (r - a / 2) + y;
    context.arc(xoff, yoff, a / 2, 0, 2 * Math.PI);
    context.stroke();
  }
}


function circle6(x, y, r, context) {
  for (var i = 0; i < 40; i++) {
    var rt1 = Math.random() * Math.PI * 2;
    var rt2 = Math.random() * Math.PI * 2;
    var cx1 = x + r * Math.cos(rt1);
    var cy1 = x + r * Math.sin(rt1);
    var cx2 = x + r * Math.cos(rt2);
    var cy2 = x + r * Math.sin(rt2);
    context.beginPath();
    context.moveTo(cx1, cy1);
    context.lineTo(cx2, cy2);
    context.stroke();
  }
}

function circle7(x, y, r, context) {
  for (var a = 0; a < 1000; a++) {
    var nr = Math.random() * r;
    var rt = Math.random() * (Math.PI * 2);
    context.beginPath();
    var xoff = Math.cos(rt) * nr + x;
    var yoff = Math.sin(rt) * nr + y;
    context.arc(xoff, yoff, 10, 0, 2 * Math.PI);
    context.stroke();
  }
}

function circle8(x, y, r, context) {
  for (var a = 0; a < 1000; a++) {
    var rt = r * Math.sqrt(Math.random());
    var theta = Math.random() * Math.PI * 2;
    context.beginPath();
    var xoff = Math.cos(theta) * rt + x;
    var yoff = Math.sin(theta) * rt + y;
    context.arc(xoff, yoff, 10, 0, 2 * Math.PI);
    context.stroke();
  }
}

function circle9(x, y, r, context) {
  for (var i = 0.4; i <= r; i += Math.random() * 10.123) {
    context.beginPath();
    context.arc(x, y, i, 0, 2 * Math.PI);
    context.stroke();
  }
}

function circle10(x, y, r, context) {
  for (var i = 0.2; i <= r; i += Math.random() * 10.123) {
    context.beginPath();
    context.arc(x, y, i, Math.random() * Math.PI, Math.random() * Math.PI * 2);
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
  for(var i=1; i<11; i++){
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
  const canvas = document.getElementById('circle1_2');
  const context = canvas.getContext('2d');

  resize(canvas);
  context.lineWidth = pixelRatio;
  context.lineJoin = 'round';
  context.textAlign = 'center';

  context.strokeStyle = 'rgba(0,0,0,0.2)';
  context.fillStyle = 'rgba(0,0,0,0.2)';

  context.save();
  context.translate(width * pixelRatio/ 2, height * pixelRatio / 2);
  circle1_2(0,-30,bodyWidth,context);
  context.font = "32px sans-serif";
  context.fillText('random point circle', 0, bodyWidth+20);
  context.restore();
}
drawAdditional();


console.log("ciao")
</script>
