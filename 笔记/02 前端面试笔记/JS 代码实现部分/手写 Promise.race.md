

```js
race(promises) {
	return new Promise((resolve, reject) => {
		if (!Array.isArray(promises)) {
			throw new TypeError('Argument is not iterable');
		}
		
		let resolved = false;
		
		for (let i = 0; i < promises.length; i++) {
			Promise.resolve(promises[i]).then(
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
```


- `Promise.race` 会立即返回 `cache` 数组中第一个完成的 promise 的结果。
- 其他未完成的 promise 会继续执行，但它们的结果不会影响 `Promise.race` 的返回值。