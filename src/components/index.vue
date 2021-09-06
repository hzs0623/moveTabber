<template>
  <Tabbar v-model="index" v-bind="$attrs">
    <TabItem v-for="item in items" :key="item[rowKey]" @change="change">
      <slot v-bind:content="item"></slot>
    </TabItem>
  </Tabbar>
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
    value: {
      type: Number,
      default: -1
    },
    items: {
      type: [Object, Array],
      default: () => {}
    },
    rowKey: {
      type: String,
      default: () => '',
      validator (value) {
        return value
      }
    }
  },
  data () {
    return {
      curIndex: 0
    }
  },
  computed: {
    index: {
      get: function () {
        return this.value
      },
      set: function (i) {
        this.curIndex = i
      }
    }
  },
  methods: {
    change (index, flag) {
      this.$emit('input', this.curIndex) // 双向绑定
      this.$emit('change', this.items[index], index, flag)
    }
  }
}
</script>
