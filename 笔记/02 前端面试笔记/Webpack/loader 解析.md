
一个 loader 就是一个 nodejs 模块，他导出的是一个函数，这个函数只有一个入参，这个参数就是一个包含资源文件内容的**字符串**，而函数的返回值就是处理后的内容。

```js
module.exports = function (source) {

    const options = this.getOptions() // 获取 webpack 配置中传来的 option

    this.callback(null, addSign(source, options.sign))

    return // return undefined 让模块自己去找 callback 要结果

}

  
function addSign(content, sign) {

    return `/** ${sign} */\n${content}`

}
```