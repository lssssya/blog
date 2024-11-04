
### 缩小文件搜索范围

- 优化loader，用 include 和 exclude

- module.noParse，不解析jQuery这种

- resolve.modules：告诉webpack可以优先从这里找

- resolve.alias：也是告诉提前告诉webpack路径 

### 减少打包文件

webpack 会打包第三方模块，增加打包尺寸

- 提取公共代码

Webpack4引入了`SplitChunksPlugin`插件进行公共模块的抽取；由于webpack4开箱即用的特性，它不用单独安装，通过`optimization.splitChunks`进行配置即可。

我们首先将使用到的第三方模块提取到一个单独的文件，这个文件包含了项目的基础运行环境，一般称为`vendors.js`，有时候项目依赖模块比较多，`vendors.js`文件会特别大，我们还可以对它进一步拆分，按照模块划分

- externals 

有一些第三方的库会从CDN引入（比如jQuery等），如果在bundle中再次打包项目就过于臃肿，我们就可以通过配置`externals`将这些库在打包的时候排除在外。

```json
{  
	externals: {    
		'jquery': "jQuery",    
		'react': 'React',    
		'react-dom': 'ReactDOM',    
		'vue': 'Vue'  
	}
}
```

这样就表示当我们遇到`require('jquery')`时，从全局变量去引用`jQuery`

- Tree Shaking

为了让Tree Shaking生效，我们需要使用ES6模块化的语法，因为ES6模块语法是静态化加载模块，它有两个特点：
1. 静态加载模块
2. 编译时加载，可以静态分析
如果是`require`，在运行时确定模块，那么将无法去分析模块是否可用，只有在编译时分析，才不会影响运行时的状态。


### 缓存

- cache-loader：将比较大的loader产生的结果缓存到磁盘，作用一般体现在第二轮
- HardSourceWebpackPlugin


### 其他

- 生产环境关闭 `productionSourceMap`、`css sourceMap`
- webpack-bundle-analyzer （插件）分析大文件
- SplitChunksPlugin
- Minification
- OptimizeCSSAssetsPlugin