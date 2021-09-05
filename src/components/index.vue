<template>
  <div class="main">
    <Tabbar v-model="index" v-bind="$attrs">
      <TabItem v-for="item in items" :key="item.label" @change="change">
        <slot v-bind:content="item"></slot>
      </TabItem>
    </Tabbar>
  </div>
</template>

<script>
import Tabbar from './Tabbar.vue'
import TabItem from './tab-item.vue'
export default {
  name: 'daesTabbar',
  components: {
    Tabbar,
    TabItem
  },
  props: {
    items: {
      type: [Object, Array],
      default: () => {}
    }
  },
  data () {
    return {
      index: 0
    }
  },
  methods: {
    change (index, flag, curIndex) {
      this.$emit('change', this.items[index], index, flag, curIndex)
    }
  }
}
</script>

<style>

</style>
