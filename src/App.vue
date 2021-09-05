<template>
  <div class="main">
    <tabbar :items="list" v-bind="options" v-slot="{ content }" >
      <div class="item">{{content.label}}</div>
    </tabbar>
  </div>
</template>

<script>
import tabbar from './components/index.vue'
export default {
  name: 'App',
  components: {
    tabbar
  },
  data () {
    return {
      index: 0,
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
        cancelableSelect: true // 可取消选中
      }
    }
  },
  methods: {
    // change (index, flag) {
    //   console.log(index, flag)
    // }
  }
}
</script>

<style>
* {
  padding: 0;
  margin: 0;
}

.main {
  border-bottom: 1px solid #eee;
  box-shadow: 0 0px 6px 1px #eee
}

.item {
  padding: 20px;
}
</style>
