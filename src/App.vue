<template>
  <div class="main">
    <Tabbar v-model="index" :items="list" :row-key="`label`" v-bind="options" v-slot="{ content }" @change="change" >
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
        invertSelect: false // 可反选
        // moveMultipleX: 1
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
