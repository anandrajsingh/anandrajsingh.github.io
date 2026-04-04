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

**Step 3: Compilation(JIT)**
JavaScript is not compiled ahead of time, but it is compiled just before execution. Most modern JavaScript engines use <b>Just-In-Time(JIT)</b> compilation. The steps involved are:
- **Intermediate Representation (IR)**: The AST is translated into an intermediate code that the JavaScript engine can further optimize and execute. The representation is platform-independent.
- **JIT Compilation**: The engine compiles this intermediate code into machine code (low-level code that your CPU can understand). This machine code can be executed directly by the system's hardware.

Some engines optimize this process by compiling the code in stages. For instance, it may first compile the code using a fast, unoptimized compiler, and if a particular part of code is run repeatedly, it recompiles that section with more optimized version.

**Step 3: Execution**
Now that JavaScript code is compiled, the engine begins execution.
- **Global Execution Context**: The engine first creates a <b>





<h2>Data Types</h2>
JavaScript data types define the type of value a variable can hold. They are divided into primitive types and reference types.

**Two Categories of Data Types**
### Primitive Types
string, number, boolean, undefined, null, symbol, bigint.</br>

They are stored by value, are immutable and are copied independently.

### Reference Types
Object, Arrays, Functions, Dates, Maps, Sets. </br>

They are stored by reference, are mutable and share same memory.



<h2>Var, let, const</h2>
They are used to declare variable in JavaScript

<h4>var</h4>

- function scoped
- Can be redeclared
- Can be reassigned
- Hoisted and initialized with undefined

<h4>let</h4>

- Block scoped
- Cannot be redeclared in the same scope
- can be reassigned
- Hoisted but placed in <b>Temporal Dead Zone</b>

<h4>const </h4>

- Block scoped
- Cannot be redeclared
- Cannot be reassigned
- Must be initialized during declaration




<h2>Hoisting</h2>
Hoisting is JavaScript's behaviour of moving variable and function declarations to the top of their scope during the memory creation phase of execution.

function declarations → hoisted completely
var variables → hoisted with undefined
let and const → hoisted but not initialized




<h2>Temporal Dead Zone</h2>
The temporal dead zone is the time between entering a scope and the actual declaration of variable declared with let and const.

During this period the variable exists but cannot be accesed, and accesing it throws a ReferenceError




<h2>Type Coercion</h2>
Type coercion is the automatic conversion of one data type to another by JavaScript during operations or comparisions.

<h4>Types of coercion:</h4>

Implicit coercion → automatic conversion
Explicit coercion → manual conversion using functions





<h2>== vs ===</h2>

== (Loose Equality)
Compares values after performing type coercion
5 == "5" → true

=== (Strict Equality)

Compares both value and type without coercion
5 === "5" → false

Moder JavaScript prefers === to avoid unexpected coercion





<h2>Function Declaration</h2>
A function declaration defines a named function using function keyword.
It is also known as function statement.
The normal way of writing a function is function statement

```
function a(){
    console.log("hello")
}
```

<h4>Characterstics:</h4>

- Fully hoisted
- Can be called before declaration



<h2>Function Expression</h2>
when an anonymous function is assigned to a variable it's called function expression.
```
var a = function (){
    console.log("hello")
}
```

<h4>Characterstics:</h4>

- Not fully hoisted
- Behaves like normal variable declaration

### Named function expression
When a named function is assigned to a variable it's called named function expression.
```
var b = function test(){
    console.log("hello")
}
```

### First Class Functions
The ability to use functions as values is known as first class functions. Like assigning it to a variable, passing as an argument/parameter or returning from a function.

### Callback Functions
The function that is passed into another function is known as callback function.


<h2>10. Arrow functions</h2>
Arrow functions provide a <b>shorter syntax for writing functions</b>.

<h4>Characterstics:</h4>

- Short syntax
- No own this binding
- Often used in callbacks




<h2>11. Parameters</h2>

<h4>Default Parameters</h4>
Default parameters allow functons to assign default values to parameters when no argument or undefined is passed

function sum(a, b=5) {}

<h4>Rest parameters</h4>
Rest parameters collect multiple arguments into a single array

function test(...args) {}

args → array of arguments


<h2>12 Spread Operator</h2>
The spread operator expands elements of an array or properties of an object

[...arr]
{...obj}

It is commonly used to:

- copy arrays
- merge arrays
- copy objects




<h2>13. Array methods</h2>

<h4>map()</h4>
map() creates a new array by transforming each element of the original array.

arr.map(x => x * 2)

<h4>filter()</h4>
filter() returns a new array containing elemnts that satisfy a condition

arr.filter(x => x > 2)


<h4>reduce()</h4>
reduce() processes an array and reduces it to a single value.

arr.reduce((acc, curr) => acc + curr, 0)

<h4>find()</h4>
find() returns the first element that satisfies a condition

arr.find(x => x > 3)

<h4>some()</h4>
some() checks if at least one element satisfies a condition.

arr.some(x => x >3)

<h4>every()</h4>
every() checks if all elements satisfy a condition

arr.every(x => x > 0)



<h2>14. Objects</h2>

<h4>Object Destructuring</h4>
Object destructuring extracts properties form an object into variables

const {name} = user

<h4>Object Spread</h4>
Object spread copies properties from one object into another

const newObj = {...obj}

This creates a shallow copy.

<h4>Property Shorthand</h4>
Property shorthand allows using a variable name directly as an object property when both names are the same.

const name = "Anand"
const user = { name }

Equivalent to:

{name: name}

<br/>


<br/>


<br/>


<h1>Part 2</h1>

<h2>Execution Context</h2>
An execution context is the environment in which JavaScript code is evalueated and executed

JavaScript creates an execution context whenever code runs

Types of Execution condtext:
1. Global Execution Context (GEC)
2. Function Execution Context (FEC)
3. Eval Execution Context (rarely used)

<h4>Execution Context Phases</h4>
<b>1. Creation Phase</b>
JavaScript prepares memory

Variables → memory allocated
Functions → stored completely
var → initialized as undefined

<b>2. Execution Phase</b>

- Code runs line by line
- Variables get real values
- Functions are executed




<h2>2. Call Stack</h2>
The call stack is a data structure used by JavaScript to keep track of execution contexts during program execution.

It follows: <b>LIFO (Last In First OUt)

<b>How it works</b>

- Function called → execution context created
- Execution context → pushed to stack
- Function finished → popped from stack


Important Points
- Top of stack = currently executing function
- Infinite recuresion -> stack overflow. Will throw error: "RangeError: Maximum call stack exceeded"



<h2>3. Scope</h2>
Scope determines where variables are accessible in a program.

Type of Scope

- Global Scope
- Function Scope
- Block Scope (let, const)



<h2>4. Scope Chain</h2>
the scope chain is the mechanism JavaScript uses to resolve variables.

When accessing a variable, JavaScript searches:

Current scope
↓
Outer scope
↓
Global scope
↓
null

<h4>Varable Shadowing</h4>
Inner variables override outer variables



<h2>5. Closures</h2>
A closure is created when a function remembers variables from its lexical scope even after the outer function has finished executing.

<h4>Common Users:</h4>

- Data encapsulation
- Private variables
- Event handlers
- Callbacks
- React hooks




<h2>6. this Binding</h2>

this refers to the object that is currently executing the funciton.

<b>This depends on how a function is called NOT where it is defined</b>

<h3>Four Binding Rules</h3>

<h4>1. Default Binding</h4>
```
function test() {
  console.log(this);
}
test();
```

Output:
IN Browser => window
In strict mode => undefined

<h4>2. Implicit Binding</h4>
obj.method()

```
const user = {
  name: "Anand",
  greet() {
    console.log(this.name);
  }
};
```

this → user

<h4>3. Explicit Binding</h4>
Using:

- call()
- apply()
- bind()

<h4>new Binding</h4>

new Constructor()

- this refers to the new object.

<b>Binding Priority</b>

- new
- bind
- call/apply
- implicit
- default

<b>Arrow Function this</b>

DO NOT have their own this. They inherit this from lexical scope

```
const obj = {
  name: "Anand",
  greet: () => console.log(this.name)
};
```
The output for the above will be undefined

<h2>Prototypes</h2>
A prototype is an object from which other objects inherit properties

Every JavaScript object has:

- [[Prototype]]

Accessible using:

- __proto__

<h4>Prototype Chain</h4>

object
↓
object.__proto__
↓
Object.prototype
↓
null


<h2>8. Constructor Functions</h2>

Before ES6 classes, objects were created using constructor functions

```
function Person(name) {
  this.name = name;
}
const p1 = new Person("Anand");
```

<b>What new Does</b>
new performs four steps:

1. Create new object {}
2. Set prototype → Constructor.prototype
3. Bind this → new object
4. Return object

Why Use Prototype Methods

- Bad: methods created for every object
- Good: methods shared via prototype



<h2>9. Promises</h2>
A Promise is an object representing the eventual completion or failure of an asynchronous operation.

<b>Promise States</b>

- Pending
- Fulfilled
- Rejected

<b>Promise Methods</b>

- .then()
- .catch()
- .finally()

<h4>Promise Chaining</h4>
Each .then() returns a new promise


<h2>10. Async / Await</h2>
async / await is syntactic sugar over promises that makes asynchronous code look synchronous

<h4>async</h4>
Always returns a promise.
```
async function test() {
  return 10;
}
```

Equivalent to Promise.resolve(10)


<h4>await</h4>
Pauses execution until a promise resolves

- const data = await fetchdata()



<h2>11. MicroTasks vs Macrotasks</h2>

<h4>Microtasks</h4>
Higher Priority

example:

- Promise.then
- Promise.catch
- queueMicrotask
- MutationObserver

<h4>Macrotasks</h4>
Lower priority

- setTimeout
- setInterval
- I/O operations
- UI rendering



<h2>12. Event Loop </h2>
The event loop is the mechanism that allows JavaScript to perorm non-blocking asynchronous operations

<h4>Event Loop Flow</h4>

Call Stack
↓
Synchronous Code
↓
Microtask Queue
↓
Macrotask Queue

<h4>Important Rule</h4>

Microtasks always execute before macrotasks