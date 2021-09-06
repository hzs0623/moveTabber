
# Daes-tab


 一个用于移动端的可触摸滑动具有回弹效果的可复用Vue组件

## Demo

> daes-tab 使用方法：

```html
<template>
  <div class="main">
    <Tabbar v-model="index" :items="list" :id="`label`" v-bind="options" v-slot="{ content }" @change="change" >
      <div class="item">{{content.label}}</div>
    </Tabbar>
  </div>
</template>

<script>
import Tabbar from './components/index.vue'
export default {
  name: 'App',
  components: {
    Tabbar
  },
  data () {
    return {
      index: 2,
      list: [
        { label: '首页' },
        { label: '推荐' },
        { label: 'Android' },
        { label: '前端' },
        { label: '后端' },
        { label: 'iOS' },
        { label: '产品' },
        { label: '设计' }
      ],
      // 配置
      options: {
        activeStyle: {
          color: 'red',
          background: 'blue'
        },
        invertSelect: true // 可反选
      }
    }
  },
  methods: {
    change (item, index, flag) {
      console.log(item, index, flag)
    }
  }
}
</script>
```

## 可选的配置项：

 配置项 | 类型 | 描述 | 默认值 
--------|---------|-------|-----
 additionalX | Number | 近似等于超出边界时最大可拖动距离 | 50px 
 reBoundExponent | Number | 惯性回弹指数(值越大，幅度越大，惯性回弹距离越长) | 10 
 sensitivity | Number | 惯性滑动时的灵敏度(值越小，阻力越大),可近似认为手松开后速度减为零所需时间 | 1000ms 
 reBoundingDuration | Number | 回弹动画duration | 360ms 
 moveAnimationSwitch | Boolean | 是否移动动画 | true
 moveMultipleX | Number | 切换移动位置倍数(设置1只会移动元素的一半)默认往居中移动 | 0
 invertSelect | Boolean | 可取消选中 | false
 activeStyle | Object | 选中元素样式 | {}


## Attribute：

 配置项 | 类型 | 描述 | 默认值 
--------|---------|-------|-----
 items | Object|Array | 列表数据 | {}
 row-key | String | 数据的Key，用来优化List的渲染；该属性是必填的。 | ''


## Methods

 事件名 | 参数 | 描述 
 --------|---------|---------
 change | item, index, flag, curIndex | 当前元素数据(flag选中状态)

 ## Scoped Slot

 name | 说明 
 --------|---------
 content | item的内容. 参数为当前元素的值

## Build Setup

```bash
# install dependencies
npm install
# or
yarn

# serve with hot reload at localhost:8080
npm run dev
# or
yarn dev

```

注意：由于slot插槽，请使用vue2.6以上版本。
