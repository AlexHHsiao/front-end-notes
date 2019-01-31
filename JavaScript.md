## Debounce and Throttle

场景： 当一个敲击可以立马搜索的输入框, 当用户一直在敲击键盘, debounce 是在用户停止输入后, 并且delay时间内没有再输入, 那么执行一次搜索。throttle 是每隔delay时间便执行一次搜索。按照中文的意思也好理解，防抖和节流，参考他们的timer写法，非常相似。
**Debounce**: only execute _**once**_ after an event stop after a certain delay time, the execute step can happen when event begin or stop.


```js
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

```js
window.addEventListener('resize', throttle(resizeHandler, 2000));

function resizeHandler(e) {
  console.log('resize');
}

// using timestamp, will execute immediately when event begain to happen, 
// and will not execute after last event trigger  
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

// using timmer, will not execute when event happen first time, 
// until delay time, and will execute after last event trigger ends
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

## Array Flatten

```js
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

## Curry

```js
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

const f = function(a,b,c) {
  return console.log([a,b,c])
}
const curried = curry(f);
curried(1)(2)(3)
```

## Deep Copy

```js
// Object.prototype.toString.call(1), [object Number]
function clone(value) {
  if (Array.isArray(value)) {
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

let obj1 = {
  a: 1,
  b: [1,3],
  c: {
    m: 1,
    n: [1,2],
    p: {
      q: 1
    }
  }
};

let obj2 = clone(obj1);
obj1.c.p = [1,2];
console.log(obj2.c.p);
```

## Array.reduce
```js
Array.reduce(accumulator, currentValue, currentIndex, array)
```
**reduce -> map + filter**

```js
// double array element, and return the values grater than 5
const arr = [1, 2, 3, 4];
const res = arr.reduce((list, num) => {
  num = num*2;
  if (num > 5) {
    list.push(num);
  }
  return list;
}, [])
```
**reduce -> array sum**

```javascript
const arr = [1, 2, 3, 4, 5];
const sum = arr.reduce((acc, num) => acc+num, 0);
```

## Promise
```js
// your simple custom promise
var p = new CustomPromise((resolve, reject) => {
  setTimeout(() => {
    resolve('A')
  }, 2000)
})

p.then(res => {console.log(res)});

function CustomPromise (fn) {
  var that = this;

  this.then = function(cb) {
    that.cb = cb;
  }
  this.resolve = function(data) {
    that.cb(data)
  }
  fn(this.resolve);
}
```
## React topics
**[props vs state](https://www.jianshu.com/p/841a8b6eab46)**
```
UI = Component(props, state)

props is the component's interface for external
state is the component's interface for internal

state:
  1. can not be changed directly, must use setState()
  2. state update is async
  3. state update is a shallow merger process  

props:
  2. props can not be changed
```