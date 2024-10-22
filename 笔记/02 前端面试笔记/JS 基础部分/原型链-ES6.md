##### ES6 的 class

对于 es6 来说，有 class 的语法糖，将工厂函数改写为 class

```js
class User {
  constructor(a, b){
	this.name = a
	this.age = b
  }
  grow(year) {
    this.age += year
  }
}
```

class 做的事情
1. 创建了 User 函数
2. 把 class 的 constructor 里面的代码放到了 User 里面
3. 然后把 class 的方法放在了 `User.prototype` 里面

##### ES6 的 extends

对于继承的写法

```js
class Admin extends User {
  constructor(a, b, c){
    // 子类的构造函数必须调用 super() 来初始化父类的构造函数
    super(a, b) 
    this.address = c
  }
  // 重写方法父类的 grow 方法
  grow(years) {
    super.grow(years) // 调用父类的 speak() 方法
  }
}
```

对于方法 `super.method()` 相当于

```js
Admin.prototype.grow = function(year){
  User.prototype.grow.call(this,year)
  // 先继承，然后再做其他事情
  // ... 
}
```

所以继承要做的事情
1. 子类Admin的构造函数希望拥有父类User构造函数的属性，所以
```js
function Admin(name,age,address){
  User.call(this,name,age)
  this.address = address
}
```
2. 然后子类继承方法
```js
// 要用 create 新建一个原型关系，而不是直接把 User.prototype 赋值给 Admin.prototype ！
// 不希望 Admin 和 User 共用一个 prototype
Admin.prototype = Object.create(User.prototype) 
// 方法都继承了
// 然后对于个别方法重写的话，就如下
Admin.prototype.grow = function(years) {
  User.prototype.grow.call(this, years)
  console.log(`he is admin, he lives in ${this.address}`)
}
```
3. 然后修改构造函数的关系
```js
// 每个函数都有 prototype 对象，里面的 constructor 指向这个函数本身， 子类的 prototype 是继承了父类的所有方法，包括 constructor。
// 所以有 Admin.prototype.constructor === User.prototype.constructor
// 这时候的 Admin.prototype.constructor 是 User 函数本身，所以要修改一下
Admin.prototype.constructor = Admin
```