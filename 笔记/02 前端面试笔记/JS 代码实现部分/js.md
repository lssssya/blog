
数组转对象

```js
const typesConvertDictionary = (array) => Object.fromEntries(array.map(item => [item.value, item.label]))
```

随机生成六位数验证码

```js
const code = Math.floor(Math.random()*1000000).toString().padStart(6, '0')  
console.log(code)
```

数组取交集

```js
const similarity = (a, b) => a.filter(item => b.includes(item))  
console.log(similarity([1, 2, 3, 4, 5], [2, 3, 4, 5, 6]))
```

RGB 颜色转 16进制颜色

```js
const RGBToHex = (r, g, b) => '#' + [r, g, b].map(item => item.toString(16).padStart(2, '0')).join('')  
console.log(RGBToHex(255, 255, 255))
```

多维数组转一维数组

```js
const deepFlatten = array => [].concat(...array.map(item => Array.isArray(item) ? deepFlatten(item) : item))  
console.log(deepFlatten([1, [2, [3, [4, [5]]]]]))
```

一个webpack 引入 UMD 模块的问题（hatom，pdfjs）
https://stackoverflow.com/questions/65446809/webpack-5-cannot-import-umd-module

```json
{
	test: 'xxx',
	loader: 'imports-loader',
	options: {
		imports: 'pure pdfjs-dist/legacy/build/pdf.js',
		type: 'commonjs'
	}
}
// 引入时
const pdfjsLib = require('pdfjs-dist/legacy/build/pdf.js')
```

控制并发函数

```js
async function asyncPool(poolLimit, array) {  
  let cache = []  
  let result = []  
  
  for (const task of array) {  
    const p = Promise.resolve()  
      .then(() => {  
        return task()  
      })  
      .then((value) => {  
        result.push(value)  
        cache.splice(cache.indexOf(p), 1)  
      })  
    cache.push(p)  
  
    if (cache.length >= poolLimit) {  
      await Promise.race(cache)  
    }  
  }  
  await Promise.all(cache)  
  return result  
}
  
// console 测试  
(async () => {  
  const tasks = [  
    () => new Promise((resolve) => {  
      setTimeout(() => {  
        console.log(`Task ${1} completed`)  
        resolve(1)  
      }, 1000)  
    }),  
    () => new Promise((resolve) => {  
      setTimeout(() => {  
        console.log(`Task ${2} completed`)  
        resolve(2)  
      }, 500)  
    }),  
    () => new Promise((resolve) => {  
      setTimeout(() => {  
        console.log(`Task ${3} completed`)  
        resolve(3)  
      }, 300)  
    }),  
    () => new Promise((resolve) => {  
      setTimeout(() => {  
        console.log(`Task ${4} completed`)  
        resolve(4)  
      }, 400)  
    })  
  ]  
  
  const results = await asyncPool(2, tasks)  
  console.log('done')  
  console.log(results)  
})()
```
