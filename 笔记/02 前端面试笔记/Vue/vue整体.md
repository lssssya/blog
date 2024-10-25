
##### Controller 和 ViewModel 区别


**Controller** 是 MVC（Model-View-Controller）架构中的一个核心概念。在 MVC 架构中，Controller 负责接收用户输入（如 HTTP 请求），处理业务逻辑，并决定如何展示数据（即选择合适的 View）。

特点：
1. **用户交互**：Controller 是应用程序与用户的接口，负责处理用户输入（如按钮点击、表单提交等）。
2. **业务逻辑**：Controller 调用 Model 层的业务逻辑来处理请求。
3. **视图选择**：Controller 决定使用哪个 View 来展示数据，并将数据传递给 View。


**ViewModel** 是 MVVM（Model-View-ViewModel）架构中的核心概念。MVVM 架构旨在将 UI 控制逻辑与业务逻辑分离，从而使 UI 更容易测试和维护。ViewModel 的职责是充当 View 和 Model 之间的桥梁。

特点：
1. **数据绑定**：ViewModel 通常包含数据和命令（方法），并与 View 实现双向数据绑定。当 ViewModel 中的数据发生变化时，View 会自动更新，反之亦然。
2. **UI 控制逻辑**：ViewModel 包含 UI 控制逻辑，如显示条件、格式化数据等，这些逻辑通常不涉及复杂的业务逻辑。
3. **松耦合**：ViewModel 与 View 之间通过数据绑定机制通信，与 Model 之间通过数据模型通信，实现了三者的松耦合。


##### 在 Vue2.x 中如何检测数组的变化？

函数劫持，重写了数组的方法，对data的数组进行了原型链重写，调用api的时候会通知依赖。

##### SSR

vue在客户端把标签渲染成html的工作放到了服务端，然后把html直接返回给了客户端。开发的时候服务端渲染只支持 beforeCreate 和 created 两个钩子


##### 为什么 data 是个函数

因为vue可以被多次复用，创建多个实例，都是同一个构造函数，那么构造函数的引用类型会指向同一个堆，所以要用函数去创造一份。


##### complier

解析模板，生成渲染模板的render，而render的作用重要为了生成 VNode

- parse：接受 template 原始模板，生成ast
- optimize：遍历ast，标记静态节点，页面刷新时减少diff对比量，提升性能
- generate 组成render 字符串，通过 new Function 的方式转换成渲染函数
