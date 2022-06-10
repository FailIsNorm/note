## MVVM

###  组件化基础

![image-20220228190550427](/Users/fintopia/Desktop/慎独/vue/image-20220228190550427.png)

数据驱动视图 

- 组件的 data 一旦变化，立刻触发视图更新

- 实现数据驱动视图第一部

  

### 核心 APi 

#### Object.defineProperty

- 监听对象，监听数组

- 复杂对象，深度监听

  ```js
  // 监听对象属性
  function observer(target, cb) {
      if(typeof target !== 'object' || target === null){
        // 不是对象或者数组
        return target
      }
    
    if(Array.isArray(target)) {
      target.__proto__ = arrProto
    }
    
   		//重新定义各个属性
    for(let key in target){
      	defineReactive(target, key, target[key], cb)
    }
  }
  // 重新定义属性，监听起来
  function defineReactive (obj, key, val, cb) {
    //支持深度监听
    if(typeof val == "object") observer (val)
    //核心 API
      Object.defineProperty(obj, key, {
          enumerable: true,
          configurable: true,
          get: ()=>{
              /*....依赖收集等....*/
              /*Github:https://github.com/answershuto*/
              return val
          },
          set:newVal=> {
              val = newVal;
              cb();/*订阅者收到消息的回调*/ // updateView() 
          }
      })
  }
   function updateView() {
     console.log('视图更新')
   }
  ```

  

  ##### 监听数组的变化

  ```js
  // 重新定义数组原型
  const oldArrayPrototype = Array.prototype
  // 创建新对象，原型指向 oldArrayPrototype, 在扩展新方法不会影响原型
  const arrProto = Object.create(oldArrayPrototype)
  console.log(arrProto.__proto__) //指向数组本身的原型
  var methodsToPatch = [
      'push',
      'pop',
      'shift',
      'unshift',
      'splice',
      'sort',
      'reverse'
    ]; 
  //重新定义每一个方法，然后去原型上找真正的方法调用，在更新视图
  methodsToPatch.forEach(methodName => {
    arrProto[methodName] = function() {
      updateView() // 触发视图更新
      oldArrayPrototype[methodName].call(this, ...arg)
    }
  })
  ```

  

  ##### 缺点

- 深度监听需要递归到底，一次性计算量大
- 「新增删除属性监听不到，所以需要 Vue $set, $delete
- 无法原生监听数组，需要特殊处理

#### Proxy

##### 	proxy 有兼容性问题

- proxy 兼容性不好，且无法 profill

  

