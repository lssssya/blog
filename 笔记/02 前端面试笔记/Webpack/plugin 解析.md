

plugin 是一个类

```js
plugins: [ 
	new HtmlWebpackPlugin({ 
		template: path.join(__dirname, 'src/index.html') 
	}), 
	new FileListPlugin('readme.md') 
]

```


类里需要定义一个 apply 方法。在安装插件时，`apply` 方法会被 Webpack `compiler` 调用一次。`apply` 方法可以接收一个 Webpack `compiler` 对象的引用，从而可以在回调函数中访问到 `compiler` 对象。

```js
class plugin {

    config:{}

    constructor(options){

        this.config = options

    }

    apply(compiler){

        compiler.hooks.done.tap('PreloadPlugin',(Compilation)=>{

            console.log('do!');

        })

    }

}
```


	tap 注册的事件，会在 webpack 上用 call() 调用。每个阶段在执行前以及执行后，都会去调用`new SyncHook().call()`这样类似的方法
