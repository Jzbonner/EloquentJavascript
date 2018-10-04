# EloquentJavascript
Fundamental Notes on the Program Structure of Javascript

### Regular Expressions Cheat Sheet 
Regular expressions are objects that represent patterns in strings. They use their own language to express these patterns.

* /abc/	A sequence of characters
* /[abc]/	Any character from a set of characters
* /[^abc]/	Any character not in a set of characters
* /[0-9]/	Any character in a range of characters
* /x+/	One or more occurrences of the pattern x
* /x+?/	One or more occurrences, nongreedy
* /x*/	Zero or more occurrences
* /x?/	Zero or one occurrence
* /x{2,4}/	Two to four occurrences
* /(abc)/	A group
* /a|b|c/	Any one of several patterns
* /\d/	Any digit character
* /\w/	An alphanumeric character (“word character”)
* /\s/	Any whitespace character
* /./	Any character except newlines
* /\b/	A word boundary
* /^/	Start of input
* /$/	End of input

> Regular expressions are a sharp tool with an awkward handle. They simplify some tasks tremendously but can quickly become unmanageable when applied to complex problems. Part of knowing how to use them is resisting the urge to try to shoehorn things that they cannot cleanly express into them.

(_Eloquent Javascript_, Ch. 9)

### The Date Class 
JavaScript has a standard class for representing dates - or rather, points in time. THe `Date` class uses a convention where both month numbers start at zero and day numbers start at one. Timestamps are always stored in the UTC time zone (Unix Time). 

Data objects provide methods such as `getFullYear`, `getMonth`, `getDate`, `getHours`, `getMinutes`, and `getSeconds`. 

### The Mechanics of Matching 
When you use `exec` or `test`, the regular expression engine looks for a match in your string by trying to match the expression first from the start of the string, then from the second character and so on until it finds a match or reaches the end of the string. 

![Diagram 1](https://github.com/Jzbonner/EloquentJavascript/blob/master/EloqJS-imgs/EloqJS%20Ch.%209%20Diagram%201.png?raw=true)

#### Backtracking 
> Backtracking also happens for repetition operators like + and *. If you match /^.*x/ against "abcxe", the * part will first try to consume the whole string. The engine will then realize that it needs an x to match the pattern. Since there is no x past the end of the string, the star operator tries to match one character less. But the matcher doesn’t find an x after abcx either, so it backtracks again, matching the star operator to just abc. Now it finds an x where it needs it and reports a successful match from positions 0 to 4.

(_Eloquent Javascript_, Ch. 9)

#### The Replace Method 
Strings have a replace method that can be used to replace part of the string with another string. The real power of using regular expressions with `replace` is that we can refer to matched groups in replacement strings. 

A regular expression has a method `test` to test whether a given string matches it. It also has a method `exec` that, when a match is found, returns an array containing all matched groups. Such an array has an `index` property that indicates where the match started. 

Regular expressions can have options, which are written after the closing slash. The `i` option makes the match case sensitive. The `g` option makes the expression global. The `y` option makes it sticky, which means that it will not search ahead and skip part of the string when looking for a match. The `u` option turns on Unicode mode, which fixes a number of problems around the handling of characters that take up to two code units. 
