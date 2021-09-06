<template>
  <div
    @click="onClick"
    :style="activeStyle"
  >
    <slot></slot>
  </div>
</template>

<script>
export default {
  data () {
    return {
      id: (this.$parent.$children.length || 1) - 1
    }
  },
  computed: {
    activeStyle () {
      return this.currentItem ? this.$parent.activeStyle : {}
    },
    currentItem () {
      return this.$parent.value === this.id
    }
  },
  methods: {
    onClick () {
      if (!this.$parent.invertSelect && this.currentItem) return
      let curId = this.id
      let flag = true

      if (this.$parent.invertSelect && this.currentItem) { // 可反选
        curId = -1
        flag = false
      }

      this.$parent.$emit('input', curId, flag) // 双向绑定
      this.$emit('change', this.id, flag)
    }
  }
}
</script>
