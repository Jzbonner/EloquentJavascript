# EloquentJavascript
Fundamental Notes on the Program Structure of Javascript

## Drawing on Canvas 
Browsers give us several ways to display graphics. The simplest way is to use styles to position and color regular DOM elements. DOM based utilities like *Scalable Vector Graphics (SVG)* are a document markup dialect that focuses on shapes rather than text. You can embed an SVG document directly in an HTML document or include it with an `<img>` tag. 

### The Canvas Element 
Canvas graphics can be drawn into a `<canvas>` element. New canvases are inherently empty and can be given height and width attributes in order to determine display size. The `<canvas>` tag is intended to allow different styles of drawing: "2d" for two-dimensional graphics and "webgl" for three dimensional graphics through the **OpenGL Interface**. For example a common use case for the `<canvas>` element would be: 
```HTML
<p>Before canvas.</p>
<canvas width="120" height="60"></canvas>
<p>After canvas.</p>
<script>
  let canvas = document.querySelector("canvas");
  let context = canvas.getContext("2d");
  context.fillStyle = "red";
  context.fillRect(10, 10, 100, 50);
</script>
```

Just like in HTML (and SVG), the coordinate system that the canvas uses puts (0,0) at the top-left corner, and the positive y-axis goes down from there. So (10,10) is 10 pixels below and to the right of the top-left corner. 

#### Lines and Surfaces 
In the canvas interface, a shape can be filled, meaning its area is given a certain color or pattern, or it can be stroked, which means a line is drawn along it's edge. The same terminology is used by SVG. 
* The `fillRect` method fills a rectangle. It takes first the x- and y- coordinates of the rectangle's top-left corner, then it's width, and then it's height. A similar method `strokeRect`, draws the outline of the rectangle 
* The `fillStyle` property controls the way shapes are filled. It can be set to a string that specifies a color, using the color notation used by CSS. The `strokeStyle` property works similarly but determines the color used for a stroke line. The width of that line is determined with the `lineWidth` property, which may contain any positive number 
_When no width or height attribute is specified, a canvas element gets a default width of 300 pixels and height of 150 pixels._

#### Paths 
A _path_ is a sequence of lines. The 2D canvas interface takes a peculiar approach to describing such a path. It is done entirely through side effects. Paths are not values that can be stored and passed around. Instead if you want to do something with a path, you make a sequence of method calls to describe it's shape: 
```HTML 
<canvas></canvas>
<script>
  let cx = document.querySelector("canvas").getContext("2d");
  cx.beginPath();
  for (let y = 10; y < 100; y += 10) {
    cx.moveTo(10, y);
    cx.lineTo(90, y);
  }
  cx.stroke();
</script>
```

###### Curves 
A path may also contain curved lines. The `quadraticCurveTo` method draws a curve to a given point. To determine the curvature of the line, the method is given a control point as well as a destination point. The `bezierCurveTo` method draws a similar kind of curve. Instead of a single control point, this has two - one for each line's endpoints. The `arc` method is a way to draw a line that curves along the edge of a circle. 

