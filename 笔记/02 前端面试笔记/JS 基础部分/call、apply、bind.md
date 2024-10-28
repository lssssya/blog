
##### call

call() 方法在使用一个指定的 this 值和若干个指定的参数值的前提下调用某个函数或方法。

干了两个事情：
1. 改变了函数的this指向，其实函数执行会照常，只是说但凡用到this的时候，会使用传给你的那个对象
2. 执行了本函数

手写的话，原理如下

```js
function foo(){

}
var bar = {
	
}
foo.call(bar)

// 等效于
bar.foo()

// 所以就可以直接把 bar 函数嫁接过来
foo.fn = this // 这个 this 就是函数当前的执行环境，嫁接到 foo 上
foo.fn() // 这就相当于 是 foo.call(bar)
delete foo.fn
```

实践

```js
Function.prototype._call(context){
	// 基础判断略

	let result = null 
	context = context || window
	const fn = Symbol('fn') // 万一上下文本身有一个名字叫 fn 的怎么办？ 用 symbol 
    context[fn] = this

	const args = [...arguments].slice(1), // 除了第一个参数
	result = context[fn](...args) // 在context上执行一次 fn，然后在 context 上把 fn 删了
	
	delete context[fn]
	return result
}
```

##### apply

- `call` 接受一个 `this` 值后跟着零个或多个参数。
- `apply` 接受一个 `this` 值后跟一个包含所有参数的数组。

其余一样

```js
Function.prototype._apply = function (context, arr) {

    let result = null
    context = context || window;
    const fn = Symbol('fn') // 万一上下文本身有一个名字叫 fn 的怎么办？ 用 symbol 
    context[fn] = this

	if(arr){  
		result = context[fn](...arr)  
	} else{  
		result = context[fn]()  
	}

  
    delete context.fn
    return result;
}
```

##### bind

`bind` 方法用于创建一个新的函数，这个新函数的 `this` 值会被永久性地绑定到 `bind` 方法的第一个参数指定的对象上。

此外，`bind` 可以预设一部分参数，这些参数在新函数被调用时会被传入。

与 `call` 和 `apply` 不同的是，`bind` 并不立即执行原函数，而是返回一个新的函数，这个新函数可以在以后的某个时刻被调用。


`bind` 方法常用于以下场景：

- **回调函数**：在事件处理程序或其他异步代码中确保正确的 `this` 上下文。
- **函数适配器**：预设一些参数，使得原始函数可以在不同上下文中重用。
- **继承**：在构造函数或原型链中重用父类的方法，同时保持正确的 `this` 指向。


```js

// bind 返回一个函数，其this指向传入的参数

Function.prototype.bind2 = function (context) {

  var fn = this
  var args = Array.prototype.slice.call(arguments, 1); // 类数组转真正数组
  // 支持 es6可以 直接  [...arguments].slice(1)

  return function () {
    return fn.apply(context, args.concat(Array.prototype.slice.call(arguments)) )
  }
}
```