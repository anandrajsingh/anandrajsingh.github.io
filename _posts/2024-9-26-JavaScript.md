**Everything in JavaScript happens inside an Execution Context**
It has two components in it:-
- Memory Component, also known as Variable component.
- Code Component, also known as Thread of Execution.

**JavaSctipt is a synchronous Single Threaded Language.**

<h3>How JavaScript runs on typical browser environment</h3>

**Step 1: JavaScript is loaded**
- **HTML parsing**: When a web page is loaded, the browser parses the HTML and upon encountering a &lt;script&gt; tag, the browser knows that it needs to execute JavaScript.
- **Fetching JavaScript**: If the script is external, the browser fetches the file. It can either download it from the web server or retrieve it from cache if file is cached locally.

**Step 2: JavaScript is parsed**
Once the JavaScript is fetched, the JavaScript Engine begins parsing the code. this process can be divided into several steps:
- **Tokenization**: The JavaScript engine breaks the code into small pieces, called tokens. These tokens are the smallest meaningful elements in the code (e.g. keywords like function, variable name, operators).
- **Parsing**: The engine takes these tokens and builds an <b>Abstract Syntax Tree(AST)</b>. This is hierarchical structure that represents the code in a form the engine can process. For example, it identifies things like function declations, loops, conditionals, etc.

