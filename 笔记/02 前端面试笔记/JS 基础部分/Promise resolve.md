

`Promise.resolve` 方法用于将一个值转换为 `Promise` 对象，或者将现有的 `Promise` 对象返回。如果传入的参数已经是 `Promise` 对象，那么 `Promise.resolve` 将直接返回这个 `Promise` 对象；如果传入的是一个非 `Promise` 的值，它会创建一个新的 `Promise` 对象，并立即将该值解析为成功状态。


`Promise.resolve` 是一个非常有用的工具，可以将不同类型的值统一为 `Promise` 对象，从而方便地进行异步操作的处理和链式调用。