## Promise实现

在传统的异步编程中，如果异步之间存在依赖关系，我们就需要通过层层嵌套回调来满足这种依赖，如果嵌套层数过多，可读性和可维护性都变得很差，产生所谓“回调地狱”，而Promise将回调嵌套改为链式调用，增加可读性和可维护性。下面我们就来一步步实现一个Promise：

#### 1. 观察者模式

我们先来看一个最简单的Promise使用：

```javascript
const p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('result')
    },
    1000);
}) 

p1.then(res => console.log(res), err => console.log(err))
```

观察这个例子，我们分析Promise的调用流程：

- `Promise`的构造方法接收一个`executor()`，在`new Promise()`时就立刻执行这个executor回调
- `executor()`内部的异步任务被放入宏/微任务队列，等待执行
- `then()`被执行，收集成功/失败回调，放入成功/失败队列
- `executor()`的异步任务被执行，触发`resolve/reject`，从成功/失败队列中取出回调依次执行

其实熟悉设计模式的同学，很容易就能意识到这是个**观察者模式**，这种`收集依赖 -> 触发通知 -> 取出依赖执行` 的方式，被广泛运用于观察者模式的实现，在Promise里，执行顺序是`then收集依赖 -> 异步触发resolve -> resolve执行依赖`。依此，我们可以勾勒出Promise的大致形状：

```js
class MyPromise {
  // 构造方法接收一个回调
  constructor(executor) {
    this._resolveQueue = []    // then收集的执行成功的回调队列
    this._rejectQueue = []     // then收集的执行失败的回调队列

    // 由于resolve/reject是在executor内部被调用, 因此需要使用箭头函数固定this指向, 否则找不到this._resolveQueue
    let _resolve = (val) => {
      // 从成功队列里取出回调依次执行
      while(this._resolveQueue.length) {
        const callback = this._resolveQueue.shift()
        callback(val)
      }
    }
    // 实现同resolve
    let _reject = (val) => {
      while(this._rejectQueue.length) {
        const callback = this._rejectQueue.shift()
        callback(val)
      }
    }
    // new Promise()时立即执行executor,并传入resolve和reject
    executor(_resolve, _reject)
  }

  // then方法,接收一个成功的回调和一个失败的回调，并push进对应队列
  then(resolveFn, rejectFn) {
    this._resolveQueue.push(resolveFn)
    this._rejectQueue.push(rejectFn)
  }
}
```

写完代码我们可以测试一下:

```javascript
const p1 = new MyPromise((resolve, reject) => {
    setTimeout(() => {
      resolve('result')
    }, 1000);
  })
  p1.then(res => console.log(res))
```

## 2. then的链式调用

补充完规范，我们接着来实现链式调用，这是Promise实现的重点和难点，我们先来看一下then是如何链式调用的：

```js
const p1 = new Promise((resolve, reject) => {
  resolve(1)
})

p1.then(res => {
    console.log(res)
    //then回调中可以return一个Promise
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve(2)
      }, 1000);
    })
  })
  .then(res => {
    console.log(res)
    //then回调中也可以return一个值
    return 3
  })
  .then(res => {
    console.log(res)
  })
```

输出

```
1
2
3
```

我们思考一下如何实现这种链式调用：

1. 显然`.then()`需要返回一个Promise，这样才能找到then方法，所以我们会把then方法的返回值包装成Promise。
2. `.then()`的回调需要拿到上一个`.then()`的返回值
3. `.then()`的回调需要顺序执行，以上面这段代码为例，虽然中间return了一个Promise，但执行顺序仍要保证是1->2->3。我们要等待当前Promise状态变更后，再执行下一个then收集的回调，这就要求我们对then的返回值分类讨论

```js
// then方法
then(resolveFn, rejectFn) {
  //return一个新的promise
  return new MyPromise((resolve, reject) => {
    //把resolveFn重新包装一下,再push进resolve执行队列,这是为了能够获取回调的返回值进行分类讨论
    const fulfilledFn = value => {
      try {
        //执行第一个(当前的)Promise的成功回调,并获取返回值
        let x = resolveFn(value)
        //分类讨论返回值,如果是Promise,那么等待Promise状态变更,否则直接resolve
        //这里resolve之后，就能被下一个.then()的回调获取到返回值，从而实现链式调用
        x instanceof MyPromise ? x.then(resolve, reject) : resolve(x)
      } catch (error) {
        reject(error)
      }
    }
    //把后续then收集的依赖都push进当前Promise的成功回调队列中(_rejectQueue), 这是为了保证顺序调用
    this._resolveQueue.push(fulfilledFn)

    //reject同理
    const rejectedFn  = error => {
      try {
        let x = rejectFn(error)
        x instanceof MyPromise ? x.then(resolve, reject) : resolve(x)
      } catch (error) {
        reject(error)
      }
    }
    this._rejectQueue.push(rejectedFn)
  })
}
```

然后我们就能测试一下链式调用：

```js
const p1 = new MyPromise((resolve, reject) => {
  setTimeout(() => {
    resolve(1)
  }, 500);
})

p1
  .then(res => {
    console.log(res)
    return 2
  })
  .then(res => {
    console.log(res)
    return 3
  })
  .then(res => {
    console.log(res)
  })

//输出 1 2 3
```

