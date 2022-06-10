# 虚拟 DOM 和 diff

- vdom 是实现 vue 和 React 的重要基石

- diff 算法是 vdom 中最核心，最关键的部分

- DOM 操作非常消耗性能，js 操作很快

  ### 用 js 模拟虚拟 DOM 结构

![image-20220310164022191](/Users/fintopia/Desktop/慎独/vue/image-20220310164022191.png)

### 		snabbdom 

https://github.com/snabbdom/snabbdom

- h函数  => 生成 vnode 的数据结构

- vnode 数据结构

- patch 函数 => 自动对比差异 node 部分

  ### vdom总结

- 用 js 模拟 DOM 结构(vnode)

- 新旧 vnode 对比，得出最小的更新范围，最后更新 dom

- 最终在数据驱动视图的模式下，有效控制 DOM 操作

  ### diff 算法

#### 概述

- diff 算法是 vnode 中最核心，最关键的部分
- diff 算法能在日常使用 Vue React 中体现出来(如 key)

diff 算法即对比，是一个广泛的概念。

![image-20220310170624878](/Users/fintopia/Desktop/慎独/vue/image-20220310170624878.png)

树的 diff 算法的时间复杂度是 O(N ^ 3)

- 第一，遍历 tree1； 第二，遍历 tree2

- 第三，排序

- 1000个节点，要计算1亿次，算法不可用

  #### 优化后时间复杂度打到 O(n)

- 只比较同一层级，不跨级比较

- tag不相同，则直接删掉重建，不再深度比较

- tag 和 key 都相同，则认为是相同节点不再深度比较

  > tag不相同，则直接删掉重建，不再深度比较

  ![image-20220310183030847](/Users/fintopia/Desktop/慎独/vue/image-20220310183030847.png)

  > tag 和 key 都相同，则认为是相同节点不再深度比较

![image-20220310183037616](/Users/fintopia/Desktop/慎独/vue/image-20220310183037616.png)

w