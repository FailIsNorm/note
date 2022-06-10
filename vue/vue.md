# vue源码分析和对原理的理解

## 	数据驱动

vue核心思想之一就是`数据驱动`，指`数据`驱动生成`视图`，通过修改数据自动实现对视图的修改。这里主要分析**模板和数据是如何渲染成最终的DOM**的。

### 	new Vue的过程

Vue 初始化主要就干了几件事情，

- 合并配置
- 初始化生命周期
- 初始化事件中心
- 初始化渲染
- 初始化 data、props、computed、watcher 等等。

###  	实例挂载

$mount方法

- Vue 不能挂载在 body、html 这样的根节点上；
- 如果没有定义 render 方法，则会把 el 或者 template 字符串转换成 render 方法
- 在 Vue 2.0 版本中所有 Vue 的组件的渲染最终都需要 render 方法，是一个“在线编译”的过程；


