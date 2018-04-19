# [:arrow_left:](https://github.com/HuanxinHu/front-end-notes/blob/master/README.md) JavaScript

> #### Debounce and Throttle
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
**Throttle**: 