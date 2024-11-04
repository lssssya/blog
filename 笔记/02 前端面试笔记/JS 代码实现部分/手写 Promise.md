

### 明确的行为定义

- **状态**：`Promise/A+` 明确规定了三种状态：`pending`（挂起）、`fulfilled`（已解决）和 `rejected`（已拒绝）。一旦 `Promise` 被解决或拒绝，它的状态就不会再改变。
- **异步性**：规范要求所有 `Promise` 的解决或拒绝操作必须异步发生，也就是说，它们应该在一个未来的任务（通常是微任务）中执行。
- **链式调用**：规范定义了 `then` 方法的行为，并确保可以进行链式调用。每个 `then` 方法都会返回一个新的 `Promise`，这样可以方便地组合多个操作。
- **错误处理**：规范还定义了如何处理错误，例如，如果 `Promise` 的解决或拒绝操作抛出了异常，那么 `Promise` 应该被拒绝，并且将这个异常作为拒绝的原因。

### 其他特性

- **延迟执行**：`Promise/A+` 规定，`then` 方法注册的回调函数应该在当前栈清空后才执行，这通常意味着它们将在下一个事件循环的开始被执行。
- **循环引用检测**：规范要求在处理 `Promise` 链时避免循环引用，即一个 `Promise` 不能解决为其自身。
- **`Promise.all` 和 `Promise.race`**：虽然 `Promise/A+` 规范本身并没有强制要求提供这些方法，但是许多 `Promise` 实现都会提供 `Promise.all` 和 `Promise.race` 方法来处理多个 `Promise` 的组合场景。



```js

/*

* pending fulfilled rejected

* fulfilled 状态会携带一个值， rejected 会携带一个错误信息

* pending 状态的时候，有一个 onFulfilled 和 onRejected

* 构造函数初始化状态、值、回调。

* then 函数入参是 onFulfilled 和 onRejected，返回一个 新的 promise

* 判断状态 决定是否立即执行 onfulfilled 和 onrejected

* */

  

new Promise((resolve, reject) => {

  setTimeout(() => {

    resolve('123')

  })

}).then(value => {

  console.log(value)

})

  

class MyPromise {

  

  constructor(callback) {

    /**

     * @type {"pending"|"fulfiiled"|"rejected"}

     */

    this.status = 'pending'

    this.result = undefined

    this.reason = undefined

    this.onResolvedCallbacks = []

    this.onRejectedCallbacks = []

  
  

    // 调用 callback 把 resolve 和 reject 传给他

    // 因为这里执行的时候 是 callback 里面执行的，是找不到这两个方法的

    callback(this.resolve.bind(this), this.reject.bind(this))

  }

  

  resolve(result) {

    if (this.status === 'pending') {

      this.status = 'fulfilled'

      this.result = result

      this.onResolvedCallbacks.forEach(fn => fn(result))

    }

  }

  

  reject(reason) {

    if (this.status === 'pending') {

      this.status = 'rejected'

      this.reason = reason

      this.onRejectedCallbacks.forEach(fn => fn(reason))

    }

  }

  

  then(onFulfilled, onRejected) {

    const newPromise = new MyPromise((resolve, reject) => {

      // 容错，看这两个是不是函数

      onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : value => value

      onRejected = typeof onRejected === 'function' ? onRejected : reason => {

        throw reason

      }

  

      if (this.status === 'fulfiiled') {

        // 由于你调用then 的时候，上面callback 函数是个异步的，

        // 所以你得延后回调，等待状态变更，不然就立即触发了。

        try {

          setTimeout(() => {

            let x = onFulfilled(this.result)

            resolvePromise(newPromise, x, resolve, reject)

          })

        } catch (e) {

          reject(e)

        }

      } else if (this.status === 'rejected') {

        try {

          setTimeout(() => {

            let x = onRejected(this.reason)

            resolvePromise(newPromise, x, resolve, reject)

          })

        } catch (e) {

          reject(e)

        }

      } else if (this.status === 'pending') {

        // 都是异步的，所以谁先谁后不一定，有可能还是pending状态

        // 所以在pending的时候把执行的任务放到对应的队列中去。

        this.onResolvedCallbacks.push(() => {

          try {

            setTimeout(() => {

              let x = onFulfilled(this.result)

              resolvePromise(newPromise, x, resolve, reject)

            })

          } catch (e) {

            reject(e)

          }

        })

        this.onRejectedCallbacks.push(() => {

          try {

            setTimeout(() => {

              let x = onRejected(this.reason)

              resolvePromise(newPromise, x, resolve, reject)

            })

          } catch (e) {

            reject(e)

          }

        })

      }

    })

    return newPromise

  }

	all(promises) {
		return new MyPromise((resolve, reject) => {
			let results = [];
			let pending = promises.length;
	
			if (promises.length === 0) {
				resolve(results);
				return;
			}
	
			promises.forEach((promise, index) => {
			    // 以防不是个promise没有then方法
				MyPromise.resolve(promise).then(
					(res) => {
						results[index] = res;
						pending--;
						if (pending === 0) {
							resolve(results);
						}
					},
					(reason) => {
						// promise.all 要求 只要有一个失败 整个整体的promise 都是失败的。
						reject(reason);
					}
				);
			});
		});
	},
	
	race(promises) {
	    return new MyPromise((resolve, reject) => {
	        if (!Array.isArray(promises)) {
	            throw new TypeError('Argument is not iterable');
	        }
	        
	        let resolved = false;
	        
	        for (let i = 0; i < promises.length; i++) {
	            MyPromise.resolve(promises[i]).then(
	                value => {
	                    if (!resolved) {
	                        resolved = true;
	                        resolve(value);
	                    }
	                },
	                reason => {
	                    if (!resolved) {
	                        resolved = true;
	                        reject(reason);
	                    }
	                }
	            );
	        }
	        
	        if (promises.length === 0) {
	            resolve(); // 如果传入的是空数组，则直接解析
	        }
	    });
}


}

  

function resolvePromise(promise2, x, resolve, reject) {

  // 不能循环调用

  if (promise2 === x) {

    return reject(new TypeError('Chaining cycle detected for promise'));

  }

  // 判断是不是 promiseA+

  if (x instanceof MyPromise) {

    if (x.status === 'pending') {

      x.then(res => resolvePromise(promise2, res, resolve, reject), reject)

    } else {

      x.then(resolve, reject)

    }

  } else if (x !== null && (typeof x === 'object' || typeof x === 'function')) {

    // 判断是是不是 对象或者函数

    // 是的话 判断是不是有then方法，并且是函数，那么就可能是其他的 promise标准 promise-like

    let then = x.then

    if (typeof then === 'function') {

      // 确保函数在 x 环境下使用

      then.call(x, res => {

        resolvePromise(promise2, res, resolve, reject)

      }, err => {

        reject(err)

      })

    } else {

      resolve(x)

    }

  } else {

    // 其他就返回值好了

    resolve(x)

  }

}

  

new MyPromise((resolve, reject) => {

  setTimeout(() => {

    resolve('123')

  })

}).then(value => {

  console.log('my promise then', value)

})

```