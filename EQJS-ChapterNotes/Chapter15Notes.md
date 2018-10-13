# EloquentJavascript
Fundamental Notes on the Program Structure of Javascript

## Event Handlers
Notifying code when an event occurs is most notably accomplished through *Event Handlers*. Browsers fo this by allowing us to register functions as *handlers* for specific events. Below is a very basic example of this concept: 
```javascript 
<p>Click this document to activate the handler.</p>
<script>
  window.addEventListener("click", () => {
    console.log("You knocked?");
  });
</script>
```

### Events and DOM Nodes 
Event Listeners are called only when the event happens in context of the object they are registered on. Giving a node an `onClick` attribute has a similar effect, but you can only assign one `onClick` handler per node with `addEventListener` you can assign as me events as necessary. 