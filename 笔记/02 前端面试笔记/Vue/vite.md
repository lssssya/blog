Vite 是一个利用现代浏览器对原生 ES 模块支持的构建工具，它跳过了打包操作，提供了快速的冷启动和即时的热模块更新，解决了传统打包工具在开发阶段的缓慢问题。

##### 原生 ESM (ESModule)

浏览器支持原生 esm，与构建用的 esbuild

Vite 在开发阶段使用原生 ES 模块加载方式，通过 `index.html` 的 `type="module"` 脚本标签直接从服务器加载模块。

```html
// index.html
<script type="module" src="./index.js"></script>
```


##### 插件

- import AutoImport from 'unplugin-auto-import/vite'
- import Components from 'unplugin-vue-components/vite'
- import Pages from 'vite-plugin-pages'、import Layouts from 'vite-plugin-vue-layouts'
- import vue from '@vitejs/plugin-vue'