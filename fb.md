###1. Flatten Array (recursively and iteratively)
###2. Explain Promise
###3. Code Exericse
```
Given input:

// could be potentially more than 3 keys in the object above
items = [
  {color: 'red', type: 'tv', age: 18},
  {color: 'silver', type: 'phone', age: 20}
  ...
]

excludes = [
  {k: 'color', v: 'silver'},
  {k: 'type', v: 'tv'},
  ....
]
function excludeItems(items, excludes) {
  excludes.forEach(pair => {
    items = items.filter(item => item[pair.k] === item[pair.v]);
  });
  return items;
}

1. Describe what this function is doing...
2. What is wrong with that function ?
3. How would you optimize it ?  
```
###4. Finding DOM Node
```
Given 2 identical DOM trees (but not equal) and one element of the first DOM tree, how would you find this element in the second DOM tree ?
(Given two identical DOM tree structures, A and B, and a node from A, find the corresponding node in B.)
```
###5.Create Emitter Class
```
/*
emitter = new Emitter();

// 1. Support subscribing to events.
sub = emitter.subscribe('event_name', callback);
sub2 = emitter.subscribe('event_name', callback2);

// 2. Support emitting events.
// This particular example should lead to the `callback` above being invoked with `foo` and `bar` as parameters.
emitter.emit('event_name', foo, bar);

// 3. Support unsubscribing existing subscriptions by releasing them.
sub.release(); // `sub` is the reference returned by `subscribe` above

*/  
```