# EloquentJavascript
Fundamental Notes on the Program Structure of Javascript

## HTTP, Forms and Browsers
A browser will make a request when we enter a URL in it's address bar. When the resulting HTML page references other files, such as images and Javascript files, those are also retrieved. A moderately complicated website can easily include anywhere from 10 to 200 resources. To be able to fetch those quickly, browsers will make several GET request simultaneously, rather than waiting for the responses one at a time. 

HTML pages may include _forms_, which allow the user to fill out information and send it to the server. Below is an example of a form:
```HTML
<form method="GET" action="example/message.html">
  <p>Name: <input type="text" name="name"></p>
  <p>Message:<br><textarea name="message"></textarea></p>
  <p><button type="submit">Send</button></p>
</form>
```

When the `<form>` element's method attribute is **GET**, the information in the form is added to the end of the action _URL_ as a __query string__. The browser might make a request to this URL: 
> GET /example/message.html?name=Jean&message=Yes%3F HTTP/1.1

### Fetch 
The interface through which browser Javascript can make HTTP request is called `fetch`. Calling `fetch` returns a promise that resolves to a `Response` object holding information about the server's response, such as its status code and its headers. Note that the promise returned by the `fetch` resolves successfully even if the server responded with an error code. 

To get at the actual content of a response, you can use its text method. Because the initial promise is resolved as soon as the response’s headers have been received and because reading the response body might take a while longer, this again returns a promise.
```javascript
fetch("example/data.txt")
  .then(resp => resp.text())
  .then(text => console.log(text));
// → This is the content of data.txt
```

A similar method, called _json_, returns a promise that resolves to the value you get when parsing the body as *JSON* or reject if it's not valid JSON. 

#### HTTP Sandboxing 
Browsers protect us by disallowing scripts to make request to other domains. Servers can include a header like this in their response to explicitly indicate to the browser that it is okay for the request to come from another domain 
`Access-Control-Allow-Origin: *`. 

### Security and HTTPS
The secure HTTP protocol, used for URLs starting with _https://_, wraps HTTP traffic in a way that makes it harder to read and tamper with. Before exchanging data, the client verifies that the server is who it claims to be by asking it to prove that it has a cryptographic certificate issued by a certificate authority that the browser recognizes. Next, all data is going over the connection is encrypted in a way that should prevent eavesdropping and tampering. 

### Form Fields 
A lot of field types use the `<input>` tag. This tag's type attribute is used to select the field's style. These are commonly used `<input>` types: 
* `text` - a single-line text field
* `password` - same as text but hides text that is typed 
* `checkbox` - an on/off switch 
* `radio` - part of a multiple choice field 
* `file` - allows the user to choose a file from their computer

> Form fields do not necessarily have to appear in a <form> tag. You can put them anywhere in a page. Such form-less fields cannot be submitted (only a form as a whole can), but when responding to input with JavaScript, we often don’t want to submit our fields normally anyway.
```HTML
<p><input type="text" value="abc"> (text)</p>
<p><input type="password" value="abc"> (password)</p>
<p><input type="checkbox" checked> (checkbox)</p>
<p><input type="radio" value="A" name="choice">
   <input type="radio" value="B" name="choice" checked>
   <input type="radio" value="C" name="choice"> (radio)</p>
<p><input type="file"> (file)</p>
```

#### Focus, Disabled Fields, Text Fields, Checkboxes and Radio Buttons, Select Fields and File Fields
For further information regarding the topics above please review the following link: [Eloquent Javascript Chapter 18](https://eloquentjavascript.net/18_http.html)

## Storing Data Client Side 
Simple HTML pages with a bit of Javascript can be a great format for "mini-applications" - small helper programs that automate basic tasks. By connecting a few form fields with event handlers, you can do anything from converting between centimeters and inches to computing passwords from a master password and a website name. Storing information on a server is the enterprise approach to web applications but at times this method can be a little overkill for when you want to construct simpler web applications. 

The `localStorage` object can be used to store data in a way that survives page reloads. This object allows you to file string values under names: 
```Javascript
localStorage.setItem("username", "marijn");
console.log(localStorage.getItem("username"));
// → marijn
localStorage.removeItem("username");
```

Sites from different domains get different storage components. That means data stored in `localStorage` by a given website can, in principle, be read only by scripts on that same site. Browsers do enforce a limit on the size of the data a site can store in `localStorage`. That restriction, along with the fact that filling up people's hard drives with junk is not profitable, prevents the feature from eating up too much space. 

The following code implements a crude note-taking application. It keeps a set of named notes and allows the user to edit notes and create new ones: 
```HTML 
Notes: <select></select> <button>Add</button><br>
<textarea style="width: 100%"></textarea>

<script>
  let list = document.querySelector("select");
  let note = document.querySelector("textarea");

  let state;
  function setState(newState) {
    list.textContent = "";
    for (let name of Object.keys(newState.notes)) {
      let option = document.createElement("option");
      option.textContent = name;
      if (newState.selected == name) option.selected = true;
      list.appendChild(option);
    }
    note.value = newState.notes[newState.selected];

    localStorage.setItem("Notes", JSON.stringify(newState));
    state = newState;
  }
  setState(JSON.parse(localStorage.getItem("Notes")) || {
    notes: {"shopping list": "Carrots\nRaisins"},
    selected: "shopping list"
  });

  list.addEventListener("change", () => {
    setState({notes: state.notes, selected: list.value});
  });
  note.addEventListener("change", () => {
    setState({
      notes: Object.assign({}, state.notes,
                           {[state.selected]: note.value}),
      selected: state.selected
    });
  });
  document.querySelector("button")
    .addEventListener("click", () => {
      let name = prompt("Note name");
      if (name) setState({
        notes: Object.assign({}, state.notes, {[name]: ""}),
        selected: name
      });
    });
</script>
```