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

#### Curves 
A path may also contain curved lines. The `quadraticCurveTo` method draws a curve to a given point. To determine the curvature of the line, the method is given a control point as well as a destination point. The `bezierCurveTo` method draws a similar kind of curve. Instead of a single control point, this has two - one for each line's endpoints. The `arc` method is a way to draw a line that curves along the edge of a circle. 

#### Drawing a Pie Chart  
To draw a pie chart, we draw a number of pie slices, each made up of an ar and a pair of lines to the center of that arc. We can compute the angle taken up by each arc by dividing a full circle (2*pi) by the total number of responses and then multiplying that number by the number of people who picked a given choice. 

#### Text 
A 2D canvas drawing context provides the methods `fillText` and `strokeText`. The latter can be useful for outlining letters, but usually `fillText` is what you will use most frequently. You can specify the size, style and font of the text with the font property j

#### Images 
In computer graphics a distinction is often made between _vector_ graphics and _bitmap_ graphics. Bitmap graphics don't specify actual shapes but rather work with pixel data. The `drawImage` method allows us to draw pixel data on a canvas. This pixel data can originate from an `<img>` element or from another canvas. 

#### Transformation 
Calling the `scale` method will cause anything drawn after it to be scaled. This method takes two parameters, one to set a horizontal scale and one to set a vertical scale. Scaling will cause everything about the drawn image, include the line width, to be stretched out or squeezed together as specified. Scaling by a negative amount will flip the picture around. 
* You can rotate subsequently drawn shapes with the `rotate` method 
* You can move subsequently drawn shapes with the `translate` method 
* You can mirror subsequently drawn shapes with the `flipHorizontally` or `flipVertically` method 

**Storing and Clearing Transformations**
The `save` and `restore` methods on the 2D canvas context do the transformation management. They conceptually keep a stack of transformation states. When you call `save`, the current state is pushed onto the stack and when you call `restore`, the state on top of the stack is taken off and used as the context's current transformation. You can also use `resetTransform` to fully reset the transformation. 

> The branch function in the following example illustrates what you can do with a function that changes the transformation and then calls a function (in this case itself), which continues drawing with the given transformation. This function draws a treelike shape by drawing a line, moving the center of the coordinate system to the end of the line, and calling itself twice—first rotated to the left and then rotated to the right. Every call reduces the length of the branch drawn, and the recursion stops when the length drops below 8
```HTML
<canvas width="600" height="300"></canvas>
<script>
  let cx = document.querySelector("canvas").getContext("2d");
  function branch(length, angle, scale) {
    cx.fillRect(0, 0, 1, length);
    if (length < 8) return;
    cx.save();
    cx.translate(0, length);
    cx.rotate(-angle);
    branch(length * scale, angle, scale);
    cx.rotate(2 * angle);
    branch(length * scale, angle, scale);
    cx.restore();
  }
  cx.translate(300, 0);
  branch(60, 0.5, 0.8);
</script>
```