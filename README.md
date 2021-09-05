
# Daes-tab


 一个用于移动端的可触摸滑动具有回弹效果的可复用Vue组件

## Demo

> daes-tab 使用方法：

```html
<daes-tab v-model="selectedId">
  <daes-tab-item
    v-for="(item, index) in items"
    :key="index">
    {{item.label}}
  </daes-tab-item>
</daes-tab>
```
```javascript
export default {
  data () {
    return {
      selectedId: 0,
      items: [
        {label: '首页'},
        {label: '推荐'},
        {label: 'Android'},
        {label: '前端'},
        {label: '后端'},
      ]
    }
  }
}
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
 activeStyle | Object | 选中元素样式 | {}
 cancelableSelect | Boolean | 可取消选中 | false


## 事件 tab-item

 事件名 | 参数 | 描述 
 --------|---------|---------
 change | index, flag, curIndex | 当前点击的元素

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
vue2.6版本以上
