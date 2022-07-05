#### 在页面背景中，实现一个可全局注入的公共水印

```js
import Vue from 'vue';
import {mapGetters} from 'vuex';

const NoWatermarkRouteNames = ['login'];

export default {
    computed: {
        ...mapGetters(['user'])
    },

    methods: {
        getBackgroundImage() {
            const {user: {name}, $route: {name: routeName}} = this;
            if (!name || NoWatermarkRouteNames.includes(routeName)) return {};

            const width = 260;
            const height = 180;
            const canvas = document.createElement('canvas');
            Object.assign(canvas, {width, height});

            const ctx = canvas.getContext('2d');
            ctx.fillStyle = 'rgba(221,221,221, .6)';
            ctx.rotate(20 * (Math.PI / 180));
            ctx.font = 'normal 14px Microsoft Yahei';
            ctx.fillText(name, 20, 10);
            ctx.font = 'normal 12px Microsoft Yahei';
            ctx.fillText(Vue.filter('dateTime')(Date.now()), 20, 30);
            const imgUrl = canvas.toDataURL();
            const background = `url(${imgUrl}) 0 0 repeat, url(${imgUrl}) ${width / 2}px ${height / 2}px repeat`;

            return {background};
        }
    }
};
```

### 在 vue 主页面的(layout)文件中使用

```vue
 <div
      class="watermark"
      :style="getBackgroundImage()"
  />
```

