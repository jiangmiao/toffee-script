## ToffeeScript

ToffeeScript is a CoffeeScript dialect with Asynchronous Grammar

ToffeeScript v1 provide easy way to handle callback, but there are some limitation, eg exception handling.
And now there is a new modern native way to handle asynchronous function.

The grammar is different from v1 to v2, so ToffeeScript v2 is not compatible with v1.
It is base on CoffeeScript v2 which support await/async. 

### Features

1. Quick define a Promise object
2. Short way to write await

### Example

ToffeeScript source code
```coffeescript
# create Promise
sleep! = (ms) ->
    setTimeout resolve, ms

countdown = (n) ->
    while n--
        console.log n
        # shortcut for await
        sleep! 1000
    0

countdown 3
```
generated JavaScript code
```javascript
// create Promise
var countdown, sleep;

sleep = function(ms) {
  return new Promise((resolve, reject) => {
    setTimeout(resolve, ms);
  });
};

countdown = async function(n) {
  while (n--) {
    console.log(n);
    // shortcut for await
    await sleep(1000);
  }
  return 0;
};

countdown(3);
```
