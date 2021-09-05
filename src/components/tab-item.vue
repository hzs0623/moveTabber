<template>
  <div
    @click="onClick"
    :style="$parent.value === id ? activeStyle : {}"
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
      return {
        color: this.$parent.activeColor
      }
    }
  },
  methods: {
    onClick () {
      // console.log(this.id)
      this.$parent.$emit('input', this.id)
    }
  }
}
</script>

<style scoped>

</style>
