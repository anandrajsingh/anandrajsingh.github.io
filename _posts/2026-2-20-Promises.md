A Promise in JavaScript is an object that represents the eventual completion (or failure) of an asynchronous operations and its resulting value. Promises are used to handle asynchronous operations more effectively than traditional callback funtions, providing a cleaner and more manageable way to deal with code that executes asynchronously, such as API calls, file I/O or timers

### Using a function that returns a promise 
```
function setTimeoutPromisified(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

function callback() {
	console.log("3 seconds have passed");
}

setTimeoutPromisified(3000).then(callback)

```

### Promisified readfile
```
function fsReadfilePromisified(filepath:string, encoding:BufferEncoding){
  return new Promise((resolve, reject) => {
    fs.readFile(filepath, encoding, (err, data) => {
      if(err){
        reject(err)
      }else{
        resolve(data)
      }
    })
  })
}

fsReadfilePromisified("a.txt", "utf-8")
  .then(function(data){
    console.log(data)
  })
  .catch(function(err){
    console.log(err)
  })
```


### Callback hell
<b>Question:</b> Write code that: logs hi after 1 second, logs hello after 3 sec of step 1, and then logs hello there after 5 seconds after step 2

<b>Solution: with callback hell</b>
```
setTimeout(function(){
  console.log("hi)
  setTimeout(function(){
    console.log("hello")
    setTimeout(function(){
      console.log("hello there")
    },5000)
  }, 3000)
},1000)
```

<b>Callback but without hell</b>
```
function stepThree(){
  console.log("hello there")
}
function stepTwo(){
  console.log("hello");
  setTimeout(stepThree, 5000)
}
function stepOne(){
  console.log("hi");
  setTimeout(stepTwo, 3000)
}
setTimeout(stepOne,1000)
```

<b>Promisified Version</b>
```
setTimeoutPromisified(1000)
  .then(() => {
    console.log("hi")
    return setTimeoutPromisified(3000)
  })
  .then(() => {
    console.log("hello")
    return setTimeoutPromisified(5000)
  })
  .then(() => {
    console.log("hello there")
  })
```

### Async await syntax
The async and await syntax in Javascript provides a way to write asynchronous code that looks and behaves like synchronous code, making it easier to read and maintain. It builds on top of Promises and allows you to avoid chaining .then() and .catch() methods while still working with asynchronous operations.

```
function setTimeoutPromisified(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function solve() {
	await setTimeoutPromisified(1000);
	console.log("hi");
	await setTimeoutPromisified(3000);
	console.log("hello");
	await setTimeoutPromisified(5000);
	console.log("hi there");
}

solve();
```