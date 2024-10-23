
```js
function User(a, b){
	let user = {}
	user.name = a
	user.age = b
	user.grow = function(year){
	    this.age += year
	}
	return user
}
```

但是这种工厂函数在每个实例中都要新开个 `.grow` 方法，所以就有新的做法就是在外面写个函数然后指过去。但是函数多了就要指很多个，所以就用 `Object.create` 方法

```js
const methods = {
	grow(year){
		this.age += year
	}
  // and other function 
} 

function User(a, b){
	let user = Object.create(methods)
	user.name = a
	user.age = b
  
	return user
}
```

```Object.create(prototype [, propertiesObject])```，它用于创建一个新的对象，该对象继承自指定的原型对象（入参prototype），并且可以自定义新对象的额外属性（入参propertiesObject）。

因为每个函数都有 `prototype` 属性，就压根不用写个 `methods` 对象了。

```js
function User(a, b){
  // 方法就用 User自带的 prototype
	let user = Object.create(User.prototype)
	user.name = a
	user.age = b
  
	return user
}
// 往自己原型链上增加公用方法
User.prototype.grow = function (year) {
	this.age += year
}
// and other function 
const lsy = User('lsy', 18)
```

已经跟构造函数 `constructor` 很近了，这时候把 `User` 变成一个构造函数，然后使用 `new` 指令。

```js
function User(a, b){
	// 由于最后是使用 new ，所以创建 user 的过程会放在new 上
	// 这里只需要定义好原型和方法即可
  
	// let user = Object.create(User.prototype)
	user.name = a
	user.age = b
	// return user
}
User.prototype.grow = function (year) {
	this.age += year
}

const lsy = new User('lsy', 18)
```

这里 `const lsy = new User('lsy', 18)` 做的事情有

1. 生成一个 user 对象
2. 设置 prototype：
	- 可以用 `let user = Object.create(A.prototype)` 实现
	- 将 user 对象的原型指定为 User ，也就是 `user.__proto__ === User.prototype`
	- 由于 user 的构造函数就是 User ，也有 `user.__proto__ === User.constructoer.prototype`
3. 用this执行构造函数（生成的新对象会绑定到函数调用的this）
4. 返回 user 对象