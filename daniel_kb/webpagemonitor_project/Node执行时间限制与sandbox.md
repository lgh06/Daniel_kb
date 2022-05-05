- https://stackoverflow.com/a/71135240/5332156

> The way to actually use cancellation is through the AbortController which is available in the browser and on Node 15+
Node reference: [https://nodejs.org/api/globals.html#class-abortcontroller](https://nodejs.org/api/globals.html#class-abortcontroller)
MDN reference: [https://developer.mozilla.org/en-US/docs/Web/API/AbortController](https://developer.mozilla.org/en-US/docs/Web/API/AbortController)
Some APIs are currently using out of the box the abort signal like fetch in the browser or setTimeout timers API in Node ([https://nodejs.org/api/timers.html#timerspromisessettimeoutdelay-value-options](https://nodejs.org/api/timers.html#timerspromisessettimeoutdelay-value-options)).
For custom functions/APIs you need to implement it by yourself but it's highly encouraged to follow the Abort signal methodology so you can chain both custom and oob functions and make use of a single signal that does not need translation


- see also https://simonplend.com/category/quick-tips/node-js-quick-tips/ blogs
- https://github.com/laverdet/isolated-vm
- https://github.com/patriksimek/vm2
- https://github.com/gf3/sandbox
- https://github.com/asvd/jailed
- https://github.com/Houfeng/safeify   https://segmentfault.com/a/1190000014533283  


## 在setTimeout内捕获error或exeption
https://stackoverflow.com/questions/41431605/how-to-handle-errors-from-settimeout-in-javascript  

## Using break to exit a function in javascript
https://flexiple.com/javascript-exit-functions/  
Using break to exit from functions in javascript is a less traditional way compared to using return. Break is mostly used to exit from loops but can also be used to exit from functions by using labels within the function.

```javascript
//javascript exit function using break
const getName = () => {

    getName: {

        console.log("I get logged");

        break getName;

        //exits the function

        console.log("I don't get logged");
    }
};
```

### JS label
https://stackoverflow.com/questions/3330193/early-exit-from-function  
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/label  

## 给promise内的resolve或reject赋值到全局变量
https://stackoverflow.com/questions/26150232/resolve-javascript-promise-outside-the-promise-constructor-scope

```javascript
var promiseResolve, promiseReject;

var promise = new Promise(function(resolve, reject){
  promiseResolve = resolve;
  promiseReject = reject;
});

promiseResolve();
```


## other
https://stackoverflow.com/questions/37624144/is-there-a-way-to-short-circuit-async-await-flow

  
Unfortunately, there is no support of `cancellable` promises so far. There are some custom implementations e.g.

**Extends/wraps a promise to be cancellable and resolvable**

```javascript

function promisify(promise) {
  let _resolve, _reject

  let wrap = new Promise(async (resolve, reject) => {
    _resolve = resolve
    _reject = reject
    let result = await promise
    resolve(result)
  })

  wrap.resolve = _resolve
  wrap.reject = _reject
    
  return wrap
}
```

**Usage: Cancel promise and stop further execution immediately after it**

```javascript
async function test() {
  // Create promise that should be resolved in 3 seconds
  let promise = new Promise(resolve => setTimeout(() => resolve('our resolved value'), 3000))
  
  // extend our promise to be cancellable
  let cancellablePromise = promisify(promise)
  
  // Cancel promise in 2 seconds.
  // if you comment this line out, then promise will be resolved.
  setTimeout(() => cancellablePromise.reject('error code'), 2000)

  // wait promise to be resolved
  let result = await cancellablePromise
  
  // this line will never be executed!
  console.log(result)
}
```

In this approach, a promise itself is executed till the end, but the caller code that awaits promise result can be 'cancelled'.