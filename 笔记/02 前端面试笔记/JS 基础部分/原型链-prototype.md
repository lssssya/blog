
##### prototype 原型

1. 对象都有私有属性 `__proto__`
2. 函数都有属性 `prototype`
3. 对象的 `__proto__` 指向该对象的构造函数的 `prototype` ，即 `obj.__proto__ === Obj.prototype`
> 原型是什么？ 原型是一个对象，通过对象的 `__proto__` 属性可以访问到这个对象，或者对象的构造函数的 `prototype` 属性也可以访问的到，而构造函数的 `prototype` 属性只是刚好叫这个名字，真正的原型是该属性对应的对象。

##### 一个继承的案例

```js
function A(){}
A.prototype.func = function(){ console.log('a') }

let a = new A()

// A 生成的实例 a 上是没有 func 属性的，但是他能访问func方法，方法放到了原型上。
```

```js
function B(){
	this.func = function(){ console.log('b') }
}

let b = new B()

// B 生成的实例 b 是有 func 属性的，方法放到了 B 的属性上，直接可以访问属性。
```

> prototype 上添加的一般是让实例共享的属性和方法，如果这些属性和方法在函数中添加，那么就会使得每个实例本身上就拥有这些属性和方法，非常的占用内存，在 prototype 上添加的话，只要让实例在它们的原型中访问就可以得到，这样就实现了继承。