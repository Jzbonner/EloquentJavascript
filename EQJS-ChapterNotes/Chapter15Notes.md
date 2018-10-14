# EloquentJavascript
Fundamental Notes on the Program Structure of Javascript

## Event Handlers
Notifying code when an event occurs is most notably accomplished through *Event Handlers*. Browsers fo this by allowing us to register functions as *handlers* for specific events. Below is a very basic example of this concept: 
```html
<p>Click this document to activate the handler.</p>
<script>
  window.addEventListener("click", () => {
    console.log("You knocked?");
  });
</script>
```

### Events and DOM Nodes 
Event Listeners are called only when the event happens in context of the object they are registered on. Giving a node an `onClick` attribute has a similar effect, but you can only assign one `onClick` handler per node with `addEventListener` you can assign as me events as necessary. Event handler functions are passed an argument, the *event object*. The object holds additional information about the event. A simple use case example of this would be: 
```html
<button>Click me any way you want</button>
<script>
  let button = document.querySelector("button");
  button.addEventListener("mousedown", event => {
    if (event.button == 0) {
      console.log("Left button");
    } else if (event.button == 1) {
      console.log("Middle button");
    } else if (event.button == 2) {
      console.log("Right button");
    };
  });
</script>
```
### Propagation
For most event types, handlers registered on nodes with children will also receive events that happen in the children. Depending on the level of interaction with the element, the most specific handler will get to go first and will *propagate* outwards from the node where it happened to that node's parent node and so on to the root of the document. At any point an event handler can call the `stopPropagation` method of the event object to prevent handlers further up from receiving the event. It is also possible to use the `target` property to cast a wide net for a specific event. Please refer to the following use case example: 
```html
<button>A</button>
<button>B</button>
<button>C</button>
<script>
  document.body.addEventListener("click", event => {
    if (event.target.nodeName == "BUTTON") {
      console.log("Clicked", event.target.textContent);
    }
  });
</script>
```

### Default Action and Key Events
For most types of events, the Javascript event handlers are called *before* the default behavior takes place. If the handler doesn't want this normal behavior to happen, typically because it has already taken care of handling the event, it can call the `preventDefault` method on the event object. 

When a key on the keyboard is pressed, your browser fires a *"keydown"* event, and upon release it fires a *"keyup"* event. Despite it's name, *"keydown"* fires not only when the key is physically pushed down. 

> Modifier keys such as shift, control, alt, and meta (command on Mac) generate key events just like normal keys. But when looking for key combinations, you can also find out whether these keys are held down by looking at the shiftKey, ctrlKey, altKey, and metaKey properties of keyboard and mouse events.

(__Eloquent Javascript__, Ch. 15)

### Pointer Events 
There are two currently widely used ways to point at things on screen: mice and touchscreens. These produce different kinds of events. 

#### Mouse Clicks, Mouse Motion, Touch Events, Scroll Events, Focus Events and Load Events 
> Pressing a key fires "keydown" and "keyup" events. Pressing a mouse button fires "mousedown", "mouseup", and "click" events. Moving the mouse fires "mousemove" events. Touchscreen interaction will result in "touchstart", "touchmove", and "touchend" events. Scrolling can be detected with the "scroll" event, and focus changes can be detected with the "focus" and "blur" events. When the document finishes loading, a "load" event fires on the window.

(__Eloquent Javascript__, Ch. 15)

### Events and the Event Loop 
In the context of the event loop, browser event handlers behave like other asynchronous notifications. They are scheduled when the event occurs but must wait for other scripts that are running to finish before they get a chance to run. If you schedule too much work, either with long-running event handlers or with lots of short-running ones, the page will become slow and cumbersome to use. 

For cases where you do want to do some time-consuming thing in the background without freezing the page, browsers provide something called *web workers*, which is a Javascript process that runs alongside the main script, on its own timeline. 

#### Timers and Debouncing 
Sometimes you need to cancel a function you have scheduled. This is done by storing the the value returned by `setTimeout` and calling `clearTimeout` on it. For example: 
```javascript 
let bombTimer = setTimeout(() => {
  console.log("BOOM!");
}, 500);

if (Math.random() < 0.5) {// 50% chance
  console.log("Defused.");
  clearTimeout(bombTimer);
}
```

If you do need to do something nontrivial in such a handler, you can use `setTimeout` to make sure you are not doing it too often. This is known as *debouncing* the event. For example if we want to space responses so that they're separated by at least a certain length of time but want to fire them during a series of events and not just afterwards, you could use something like this: 
```html
<script>
  let scheduled = null;
  window.addEventListener("mousemove", event => {
    if (!scheduled) {
      setTimeout(() => {
        document.body.textContent =
          `Mouse at ${scheduled.pageX}, ${scheduled.pageY}`;
        scheduled = null;
      }, 250);
    }
    scheduled = event;
  });
</script>
```


