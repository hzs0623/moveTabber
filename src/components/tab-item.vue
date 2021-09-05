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
      if (!this.$parent.cancelableSelect && this.currentItem) return
      let curId = this.id
      let flag = true
      if (this.$parent.cancelableSelect && this.currentItem) {
        curId = -1
        flag = false
      }
      this.$parent.$emit('input', curId, flag)
      this.$emit('change', this.id, flag, curId)
    }
  }
}
</script>

<style scoped>

</style>
