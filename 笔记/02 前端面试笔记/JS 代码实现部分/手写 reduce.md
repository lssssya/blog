
```js

Array.prototype.reduce = (fn, initialValue) => {
	const arr = this
	let total = initialValue || arr[0];
	for (let i = 1; i < arr.length; i++) {
		total = fn(total, arr[i], i, arr);
	}
	return total
}

```