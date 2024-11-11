
```js

all(promises) {
	return new Promise((resolve, reject) => {
		let results = [];
		let pending = promises.length;

		if (promises.length === 0) {
			resolve(results);
			return;
		}

		promises.forEach((promise, index) => {
			// 以防不是个promise没有then方法
			Promise.resolve(promise).then(
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
}
	
```