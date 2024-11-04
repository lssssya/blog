
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
async function asyncPool (poolLimit, array, fn) {  
  const ret = []  
  let executing = []  
  
  for (const item of array) {  
    // 把所有 array 的任务封装到一个各自的promise 进行控制，压到微任务里轮到他了就 fn(item) 执行掉。  
    const p = Promise.resolve().then(() => fn(item))  
    ret.push(p)  
    // then 里面会把 executing 里的剔除一个，空出可执行内容  
    const e = p.then(() => executing.splice(executing.indexOf(e), 1))  
    executing.push(e)  
  
    if (executing.length >= poolLimit) {  
      await Promise.race(executing)  
    }  
  }  
  await Promise.all(ret)  
  return ret  
}  

// console 测试
(async () => {  
  const tasks = Array.from({ length: 10 }, (_, i) => () =>  
    new Promise((resolve) => {  
      const random = Math.random() * 1000 * (i)  
      console.log(i,random)  
      setTimeout(() => {  
        console.log(`Task ${ i } completed`)  
        resolve()  
      }, random)  
    })  
  )  
  const results = await asyncPool(3, tasks, (task) => task())  
  console.log('done')  
})()
```
