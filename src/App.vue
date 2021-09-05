<template>
  <div>
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
      index: 0,
      list: [
        { label: '首页' },
        { label: '推荐' },
        { label: 'Android' },
        { label: '前端' },
        { label: '后端' },
        { label: 'iOS' },
        { label: '产品' },
        { label: '人工智能' },
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

.item {
  padding: 20px;
}
</style>
