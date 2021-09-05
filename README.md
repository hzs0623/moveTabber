
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
 labelKey | String | 指定item的文字部分在item对象中的key | label 
 lineWidth | Number | fixBottom为false时tabbar底部滑条高度 | 1px 
 activeColor | String | 激活状态下字体以及滑条颜色 | red 
 additionalX | Number | 近似等于超出边界时最大可拖动距离 | 50px 
 reBoundExponent | Number | 惯性回弹指数(值越大，幅度越大，惯性回弹距离越长) | 10 
 sensitivity | Number | 惯性滑动时的灵敏度(值越小，阻力越大),可近似认为手松开后速度减为零所需时间 | 1000ms 
 reBoundingDuration | Number | 回弹动画duration | 360ms 


## 事件

 事件名 | 参数 | 描述 
 --------|---------|---------
 change | item, index | 当前点击的item以及索引值

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

