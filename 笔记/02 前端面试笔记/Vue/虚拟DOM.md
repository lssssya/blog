
虚拟DOM是对真实DOM的一层抽象，它是一颗 js对象树，用对象的属性描述节点，最后通过渲染器(renderer)将虚拟DOM渲染为真实DOM。

### 虚拟DOM是怎么变为真实DOM的

每一次DOM更新流程，Vue会用**patch**函数，对新老节点进行判断，执行创建/销毁节点。通过patchVnode函数和diff算法，新旧Vnode diff比较，**只更新有差异的部分**

在页面首次渲染的时候会调用patch创建新的VNode，不会进行深层次的比较

每个组件对应一个Watcher实例，当数据变化，会触发setter通过notify通知Watcher，对应的Watcher会通知更新并执行更新函数，它会执行render函数获取新的虚拟DOM，然后执行patch比较新旧Vnode，得到有差异的部分；最后根据有差异的部分更新视图

