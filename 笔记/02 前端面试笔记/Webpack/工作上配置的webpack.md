
默认babel-loader会忽略node_modules中的文件

但是dolphin-plugin-tools用了es6的语法, 配置对其显示转译
配合babel sourceType: 'unambiguous' 来使用 https://github.com/babel/babel/issues/9187,

```js

transpileDependencies: [
    'dolphin-plugin-tools',
    /@hui-pro/
],

```