### 电池状态：navigator.getBattery()

这个 `api` 返回的是一个 `promise` 对象，会给出一个 `BatteryManager` 对象

### 属性

| key                     | 描述                     | 备注       |
| ----------------------- | ------------------------ | ---------- |
| charging                | 是否在充电               | 可读属性   |
| chargingTime            | 若在充电，还需充电时间   | 可读属性   |
| dischargingTime         | 剩余电量                 | 可读属性   |
| level                   | 剩余电量百分数           | 可读属性   |
| onchargingchange        | 监听充电状态的改变       | 可监听事件 |
| onchargingtimechange    | 监听充电时间的改变       | 可监听事件 |
| ondischargingtimechange | 监听电池可用时间的改变   | 可监听事件 |
| onlevelchange           | 监听剩余电量百分数的改变 | 可监听事件 |

```js
getBatteryInfo() {
          // let that = this
          navigator.getBattery().then((battery) => {
              console.log(battery) //conso
              // that.onchargingchange()
              battery.addEventListener('chargingchange', function() {
                  if(battery.charging) {
                      alert(battery.charging)
                  }else{
                      alert(battery.level)
                  }
              })
          })
      }
```

