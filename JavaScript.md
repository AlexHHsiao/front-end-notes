# JavaScript

> #### Debounce and Throttle
**Debounce**: only execute _**once**_ after an event stop after a certain delay time, the execute step can happen when event begin or stop.
```javascript
window.addEventListener('resize', debounce(resizeHandler, 2000));

function resizeHandler(e){
  console.log('resize');
}

//simle debounce
function debounce(fn, delay) {
  let timer = null;

}
```