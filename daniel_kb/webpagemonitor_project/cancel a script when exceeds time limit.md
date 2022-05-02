https://stackoverflow.com/a/71135240/5332156

> The way to actually use cancellation is through the AbortController which is available in the browser and on Node 15+
Node reference: [https://nodejs.org/api/globals.html#class-abortcontroller](https://nodejs.org/api/globals.html#class-abortcontroller)
MDN reference: [https://developer.mozilla.org/en-US/docs/Web/API/AbortController](https://developer.mozilla.org/en-US/docs/Web/API/AbortController)
Some APIs are currently using out of the box the abort signal like fetch in the browser or setTimeout timers API in Node ([https://nodejs.org/api/timers.html#timerspromisessettimeoutdelay-value-options](https://nodejs.org/api/timers.html#timerspromisessettimeoutdelay-value-options)).
For custom functions/APIs you need to implement it by yourself but it's highly encouraged to follow the Abort signal methodology so you can chain both custom and oob functions and make use of a single signal that does not need translation


see also https://simonplend.com/category/quick-tips/node-js-quick-tips/ blogs