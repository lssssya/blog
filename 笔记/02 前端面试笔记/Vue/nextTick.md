
vue 监听到数据变化的时候不会立即去更新dom，而是开启了一个任务队列，缓存这个事件循环中所有的数据变更，从而减少dom变更次数。

> nextTick 想要操作 基于最新数据生成的dom时，就把这个操作放到 nextTick 的回调函数里面。


### 实现原理

nextTick 把传入的回调函数包装到异步任务里面。里面有宏任务和微任务，为了尽快执行，会优先放到微任务里面。

提供了4个异步方法

- Promise.then 
- MutationObserver
- setImmediate
- setTimeout
