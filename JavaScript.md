# [:arrow_left:](https://github.com/HuanxinHu/front-end-notes/blob/master/README.md) JavaScript

> ### Debounce and Throttle
Refs: [Array flatten, Currying, Debounce, throttle, deepCopy.](https://github.com/lensh/blog/issues/1)

场景： 当一个敲击可以立马搜索的输入框, 当用户一直在敲击键盘, debounce 是在用户停止输入后, 并且delay时间内没有再输入, 那么执行一次搜索。throttle 是每隔delay时间便执行一次搜索。

**Debounce**: only execute _**once**_ after an event stop after a certain delay time, the execute step can happen when event begin or stop.

```javascript
window.addEventListener('resize', debounce(resizeHandler, 2000));

function resizeHandler(e) {
  console.log('resize');
}

// simle debounce
function debounce(fn, delay) {
  let timer = null;

  return function() {
    let context = this;
    let args = arguments;

    if (timer) clearTimeout(timer);
    timer = setTimeout(() => {
      fn.apply(context, args);
    }, delay);
  }
}

// debounce with immediate
function debounce(fn, delay, immediate) {
  let timer = null;

  return function() {
    let context = this;
    let args = arguments;

    if (timer) clearTimeout(timer);

    if (immediate) {
      let doNow = !timer;
      timer = setTimeout(() => {
        timer = null;
      }, delay);
      if (doNow) {
        fn.apply(context, args);
      }
    } else {
      timer = setTimeout(() => {
        fn.apply(context, args);
      }, delay);
    }
  }
}
```
**Throttle**: when event happening, execute handler every delay time.

```javascript
window.addEventListener('resize', throttle(resizeHandler, 2000));

function resizeHandler(e) {
  console.log('resize');
}

// using timestamp, will execute immediately when event begain to happen, and will not execute after last event trigger  
function throttle(fn, delay) {
  let pre = Date.now();

  return function() {
    let context = this;
    let args = arguments;
    let now = Date.now();
    
    if (now - pre >= delay) {
      fn.apply(context, args);
      pre = Date.now();
    }
  }
}

// using timmer, will not execute when event happen first time, until delay time, and will execute after last event trigger ends
function throttle(fn, delay) {
  let timer = null;

  return function() {
    let context = this;
    let args = arguments;

    if (!timer) {
      timer = setTimeout(() => {
        fn.apply(context, args);
        timer = null;
      }, delay);
    }
  }
}
```

> ### Array Flatten

```javascript
function flatten(array) {
  let res = [];
  for(let i = 0; i < array.length; i++){
    let item = array[i];
    if (Array.isArray(item)) {
      res.push(...flatten(item));
    } else {
      res.push(item);
    }
  }
  return res;
}

console.log(flatten([1,2,[3,4],[5,[6,[7,[8], 9]]]]));
```

> ### Curry

```javascript
function curry(func) {
  let l = func.length // Function.length return argumens length function expected
  return function curried() {
    let args = Array.from(arguments);
    if (args.length < l) {
      return function() {
        let argsInner = Array.from(arguments);
        return curried.apply(this, [...args, ...argsInner]);
      }
    } else {
      func.apply(this, args);
    }
  }
}

var f = function(a,b,c) {
  return console.log([a,b,c])
}
var curried = curry(f);
curried(1)(2)(3)
```

> ### Deep Copy

```javascript
// Object.prototype.toString.call(1), [object Number]
function clone(value) {
  if(Array.isArray(value)) {
    let res = [];
    for (let i = 0; i < value.length; i++) {
      res.push(clone(value[i]));
    }
    return res;
  } else if (Object.prototype.toString.call(value) === '[object Object]') {
    let res = {};
    Object.keys(value).forEach(key => {
      res[key] = clone(value[key]);
    });
    return res;
  } else {
    return value;
  }
}
```