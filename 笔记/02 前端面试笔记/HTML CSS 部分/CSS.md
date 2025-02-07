### 盒模型介绍

标准盒模型 content-box： 宽高由content
ie盒模型 border-box：宽高由content+padding+border

### css 选择器和优先级

优先级是由 `A` 、`B`、`C`、`D` 的值来决定的，其中它们的值计算规则如下：

1. 如果存在内联样式，那么 `A = 1`, 否则 `A = 0`;
2. `B` 的值等于 `ID选择器` 出现的次数;
3. `C` 的值等于 `类选择器` 和 `属性选择器` 和 `伪类` 出现的总次数;
4. `D` 的值等于 `标签选择器` 和 `伪元素` 出现的总次数 。
```
#nav-global > ul > li > a.nav-link
```
**比较规则是: 从左往右依次进行比较 ，较大者胜出，如果相等，则继续往右移动一位进行比较 。如果4位全部相等，则后面的会覆盖前面的**



### 对 BFC 的理解

块级格式上下文，bfc。
https://juejin.cn/post/6960866014384881671

`BFC` 包含创建它的元素的所有子元素，但是不包括创建了新的 `BFC` 的子元素的内部元素。

简单来说，子元素如果又创建了一个新的 `BFC`，那么它里面的内容就不属于上一个 `BFC` 了，这体现了 `BFC` **隔离** 的思想


### flex 布局

这里有个小问题，很多时候我们会用到 `flex: 1` ，它具体包含了以下的意思：

- `flex-grow: 1` ：该属性默认为 `0` ，如果存在剩余空间，元素也不放大。设置为 `1`  代表会放大。
- `flex-shrink: 1` ：该属性默认为 `1` ，如果空间不足，元素缩小。
- `flex-basis: 0%` ：该属性定义在分配多余空间之前，元素占据的主轴空间。浏览器就是根据这个属性来**计算是否有多余空间**的。默认值为 `auto` ，即项目本身大小。设置为 `0%`  之后，因为有 `flex-grow`  和 `flex-shrink`  的设置会自动放大或缩小。

### grid 布局