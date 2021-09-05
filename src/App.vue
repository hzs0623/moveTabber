<template>
  <div class="main">
    <Tabber v-model="index" :activeColor="`blue`">
      <TabItem v-for="item in list" :key="item.label">
        <div class="item">{{ item.label }}</div>
      </TabItem>
    </Tabber>
  </div>
</template>

<script>
import Tabber from './components/Tabbar.vue'
import TabItem from './components/tab-item.vue'
export default {
  name: 'App',
  components: {
    Tabber,
    TabItem
  },
  data () {
    return {
      index: -1,
      list: [
        { label: '首页' },
        { label: '推荐' },
        { label: 'Android' },
        { label: '前端' },
        { label: '后端' },
        { label: 'iOS' },
        { label: '产品' },
        { label: '人工智能' },
        { label: '人工智能1' },
        { label: '人工智能2' },
        { label: '人工智能3' },
        { label: '人工智能13' },
        { label: '人工智能233' },
        { label: '人工智能4243' },
        { label: '人工43' },
        { label: '人工智4能3' },
        { label: '人工2智能3' },
        { label: '设计' }
      ]
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
