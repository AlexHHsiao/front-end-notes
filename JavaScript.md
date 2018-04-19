# [:arrow_left:](https://github.com/HuanxinHu/front-end-notes/blob/master/README.md) JavaScript

> ### Debounce and Throttle
Refs: [数组扁平化， 柯里化，防抖， 节流，对象拷贝](https://github.com/lensh/blog/issues/1)
场景： 当一个敲击可以立马搜索的输入框， 当用户一直在敲击键盘， debounce 是在用户停止输入后，并且delay时间内没有再输入，那么执行一次搜索。throttle 是每隔delay时间便执行一次搜索。

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

// using timestamp
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
```