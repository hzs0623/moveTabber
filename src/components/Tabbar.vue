<template>
  <div class="daes-tabbar"
    ref="viewArea">
    <div class="daes-tab-list"
         :style="style"
         ref="list">
      <slot></slot>
    </div>
  </div>
</template>

<script>

export default {
  name: 'daesTabbar',

  props: {
    lineWidth: {
      type: Number,
      default: 3
    },
    activeColor: {
      type: String,
      default: 'red'
    },
    value: {
      type: Number,
      default: 0
    },

    // 近似等于超出边界时最大可拖动距离(px);
    additionalX: {
      type: Number,
      default: 50
    },
    // 灵敏度(惯性滑动时的灵敏度,值越小，阻力越大),可近似认为速度减为零所需的时间(ms);
    sensitivity: {
      type: Number,
      default: 1000,
      validator (value) {
        return value > 0
      }
    },
    // 回弹过程duration;
    reBoundingDuration: {
      type: Number,
      default: 360
    },
    // 惯性回弹指数(值越大，幅度越大，惯性回弹距离越长);
    reBoundExponent: {
      type: Number,
      default: 10,
      validator (value) {
        return value > 0
      }
    }
  },

  data () {
    return {
      translateX: 0,
      startX: 0,
      lastX: 0,
      currentX: 0,
      touching: false, // 是否处于touch状态;
      reBounding: false, // 是否处于回弹过程;
      startMoveTime: 0,
      endMoveTime: 0,
      inertiaFrame: 0, // 标识
      speed: 0, // 滑动速度(正常滑动时一般不会超过10);
      acceleration: 0, // 惯性滑动加速度;
      frameStartTime: 0,
      frameEndTime: 0,
      frameTime: 16.7 // 每个动画帧的ms数
    }
  },

  computed: {
    style () {
      return {
        transitionTimingFunction: this.transitionTimingFunction,
        transitionDuration: `${this.transitionDuration}ms`,
        transform: `translate3d(${this.translateX}px, 0px, 0px)`
      }
    },
    transitionDuration () {
      if (this.touching || (!this.reBounding && !this.touching)) {
        return 0
      }
      if (this.reBounding && !this.touching) {
        return this.reBoundingDuration
      }
    },
    transitionTimingFunction () {
      return this.reBounding ? 'cubic-bezier(0.25, 0.46, 0.45, 0.94)' : 'cubic-bezier(0.1, 0.57, 0.1, 1)'
    },
    // 可视区宽度;
    viewAreaWidth () {
      return this.$refs.viewArea.offsetWidth
    },
    // 可视区与可滑动元素宽度差值;
    listWidth () {
      return this.$refs.list.offsetWidth - this.viewAreaWidth
    },
    // 是否向左惯性滚动;
    isMoveLeft () {
      return this.currentX <= this.startX
    },
    isMoveRight () {
      return this.currentX >= this.startX
    }
  },

  watch: {
    value (newVal) {
      if (newVal < 0) return
      this.checkPosition()
    }
  },

  mounted () {
    this.bindEvent()
    // this.checkPosition()
  },

  destoryed () {
    this.removeEvent()
  },

  methods: {
    // start
    handleTouchStart (event) {
      event.stopPropagation()
      cancelAnimationFrame(this.inertiaFrame)
      this.lastX = event.touches[0].clientX
    },

    // move
    handleTouchMove (event) {
      if (this.listWidth <= 0) return
      const { touches, timeStamp } = event
      event.preventDefault()
      event.stopPropagation()
      this.startX = this.lastX
      this.currentX = touches[0].clientX
      this.moveFollowTouch()
      this.touching = true // 触摸阶段

      // 记录每次开始与结束时间
      this.startMoveTime = this.endMoveTime
      this.endMoveTime = timeStamp // 每次触发touchmove事件的时间戳;
    },

    // end
    handleTouchEnd (event) {
      this.touching = false // 触摸结束
      if (this.checkReboundX()) {
        cancelAnimationFrame(this.inertiaFrame)
        return
      }
      let silenceTime = event.timeStamp - this.endMoveTime // 触摸持续时间
      if (silenceTime > 100) return // 停顿时间超过100ms不产生惯性滑动;
      let timeStamp = this.endMoveTime - this.startMoveTime // 快速过程时间
      timeStamp = timeStamp > 0 ? timeStamp : 8
      this.speed = (this.lastX - this.startX) / timeStamp // 初滑动速度
      this.acceleration = this.speed / this.sensitivity // 惯性滑动加速度;
      this.frameStartTime = new Date().getTime() // 记录开始时间戳
      this.inertiaFrame = requestAnimationFrame(this.moveByInertia)
    },

    // 如果需要回弹则进行回弹操作并返回true;
    checkReboundX () {
      this.reBounding = false
      let flag = false
      if (this.translateX + this.listWidth < 0) { // 左边复位
        this.reBounding = true // 回弹过程
        this.translateX = -this.listWidth
        flag = true
      }

      if (this.translateX > 0) { // 右边滑动复位
        this.reBounding = true // 回弹过程
        this.translateX = 0
        flag = true
      }
      return flag
    },

    bindEvent () {
      this.$el.addEventListener('touchstart', this.handleTouchStart, false)
      this.$el.addEventListener('touchmove', this.handleTouchMove, false)
      this.$el.addEventListener('touchend', this.handleTouchEnd, false)
    },

    removeEvent () {
      this.$el.removeEventListener('touchstart', this.handleTouchStart)
      this.$el.removeEventListener('touchmove', this.handleTouchMove)
      this.$el.removeEventListener('touchend', this.handleTouchEnd)
    },

    // touch拖动
    moveFollowTouch () {
      const curX = this.translateX + this.listWidth
      const moveX = this.currentX - this.lastX // 每次移动距离

      if (this.isMoveLeft) { // 向左拖动
        // eslint-disable-next-line no-mixed-operators
        if (this.translateX <= 0 && curX > 0 || this.translateX > 0) {
          this.translateX += moveX // 每次移动的距离
        } else if (curX <= 0) {
          const outX = Math.abs(this.translateX + this.listWidth) // 超出边界的值
          this.translateX += this.additionalX * moveX / (this.viewAreaWidth + outX)
        }
      } else { // 向右拖动
        if (this.translateX >= 0) {
          const outX = this.viewAreaWidth + this.translateX
          this.translateX += this.additionalX * moveX / outX
        } else if ((this.translateX <= 0 && curX >= 0) || curX <= 0) {
          this.translateX += moveX // 每次移动的距离
        }
      }
      this.lastX = this.currentX
    },

    // 惯性滑动
    moveByInertia () {
      this.frameEndTime = new Date().getTime()
      this.frameTime = this.frameEndTime - this.frameStartTime // 是否可以丢到计算属性里面？
      if (this.isMoveLeft) { // 向左惯性滑动;
        if (this.translateX <= -this.listWidth) { // 超出边界的过程;
          // 加速度指数变化;
          this.acceleration *= (this.reBoundExponent + Math.abs(this.translateX + this.listWidth)) / this.reBoundExponent
          this.speed = Math.min(this.speed - this.acceleration, 0) // 为避免减速过程过短，此处加速度没有乘上frameTime;
        } else {
          this.speed = Math.min(this.speed - this.acceleration * this.frameTime, 0) // 随每次加载速度越来越小
        }
      } else {
        if (this.translateX >= 0) {
          this.acceleration *= (this.reBoundExponent + this.translateX) / this.reBoundExponent
          this.speed = Math.max(this.speed - this.acceleration, 0)
        } else {
          this.speed = Math.max(this.speed - this.acceleration * this.frameTime, 0)
        }
      }

      this.translateX += this.speed * this.frameTime / 2
      if (Math.abs(this.speed) <= 0.001) { // 结束递归, 有可能超过边界 执行回弹
        this.checkReboundX()
        return
      }
      this.frameStartTime = this.frameEndTime // 当前时间给下一次开始
      this.inertiaFrame = requestAnimationFrame(this.moveByInertia) // 递归加速
    },

    // 点击切换item时，调整位置使当前item尽可能往中间显示
    checkPosition () {
      if (!this.$children.length || this.value < 0) return
      if (this.$children.length <= this.value) return
      const activeItem = this.$children[this.value].$el
      const offsetLeft = activeItem.offsetLeft
      const half = (this.viewAreaWidth - activeItem.offsetWidth) / 2
      let changeX = 0
      const absTransX = Math.abs(this.translateX)
      if (offsetLeft <= absTransX + half) { // item偏左，需要往右移
        let pageX = offsetLeft + this.translateX
        changeX = half - pageX
      } else { // item偏右，需要往左移
        changeX = -(offsetLeft - absTransX - half)
      }
      let lastX = changeX + this.translateX
      // 两种边界情况
      lastX > 0 && (lastX = 0)
      lastX < -this.listWidth && (lastX = -this.listWidth)
      this.reBounding = true
      this.translateX = lastX
    }
  }
}
</script>

<style scoped>
.daes-tabbar {
  display: flex;
  position: relative;
  overflow: hidden;
  width: 100%;
}

.daes-tab-list {
  position: relative;
  box-sizing: border-box;
  display: flex;
  flex-flow: row nowrap;
  flex-shrink: 0;
  min-width: 100%;
}
</style>
