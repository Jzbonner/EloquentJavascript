# EloquentJavascript
Fundamental Notes on the Program Structure of Javascript

## The Document Object Model 
HTML documents have a particular structure that browsers' use to provide a visual representation of the encoded HTML. This Document Object Model or (DOM) can be broken down as follows: 

![Diagram1](https://eloquentjavascript.net/img/html-boxes.svg)

## Trees 
We call a data structure a *tree* when it has branching structure, has no cycles and has a single, well defined root. In addition to representing recursive structures such as HTML documents or programs, they are often used to maintain sorted sets of data because elements can usually be found or inserted more efficiently in a tree than in a flat array. 

### Moving through the Tree 
When dealing with nested data structures, recursive functions are very useful. For example the following function scans a document for text nodes containing a given string and returns `true` when it has found one: 

```javascript
function talksAbout(node, string) {
  if (node.nodeType == Node.ELEMENT_NODE) {
    for (let i = 0; i < node.childNodes.length; i++) {
      if (talksAbout(node.childNodes[i], string)) {
        return true;
      }
    }
    return false;
  } else if (node.nodeType == Node.TEXT_NODE) {
    return node.nodeValue.indexOf(string) > -1;
  }
}

console.log(talksAbout(document.body, "book"));
// → true
```
You can access attributes of a document by using methods of the `document.body` object. For instance: 

```javascript 
let link = document.body.getElementsByTagName("a")[0];
console.log(link.href);
```

### Further Information
For a deeper look into Positioning and Animating, Query Selectors, Cascading Styles, Styling, Layout, DOM Manipulation, Attributes, and Creating Nodes please refer to [Ch. 14 of Eloquent Javascript](https://eloquentjavascript.net/14_dom.html). 

#### Summary 
> JavaScript programs may inspect and interfere with the document that the browser is displaying through a data structure called the DOM. This data structure represents the browser’s model of the document, and a JavaScript program can modify it to change the visible document. The DOM is organized like a tree, in which elements are arranged hierarchically according to the structure of the document. The objects representing elements have properties such as parentNode and childNodes, which can be used to navigate through this tree. The way a document is displayed can be influenced by styling, both by attaching styles to nodes directly and by defining rules that match certain nodes. There are many different style properties, such as color or display. JavaScript code can manipulate an element’s style directly through its style property.