
```js

function instanceof (target, origin) {
	if (typeof target !== "object" || target === null) return false
	if (typeof origin !== "function") throw new TypeError("origin must be function");
	
	let proto = target.__proto__
	while (proto) {
		if (proto === origin.prototype) return true
		proto = proto.__proto__
	}
	return false
}

```

  
