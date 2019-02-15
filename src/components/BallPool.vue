<template>
<div id="ball-pool" ref="ball-pool">
  <div 
    v-for="(value, index) in balls" 
    :class="['ball', ballClass(value)]" 
    :key="value" 
    :style="{
      width: `${diameter}px`,
      height: `${diameter}px`,
      left: `${randomXArray[index]}px`,
      top: `${-diameter}px`,
    }">
    <span :class="['text', textClass]" @animationend="animationend">{{toString(value)}}</span>
  </div>
</div>
</template>

<script>
export default {
  props: {
    running: {
      type: Boolean,
      default: false,
    },
    matchedBalls: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      balls: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16],
      randomXArray: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      diameter: 0,
    }
  },
  computed: {
    textClass() {
      return this.running ? 'text-animation' : '';
    }
  },
  mounted() {
    this.setDiameter()
    window.addEventListener('resize', this.setDiameter)
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.setDiameter)
  },
  watch: {
    running(newV, oldV) {
      if (newV && !oldV) {
        const rect = this.$refs['ball-pool'].getBoundingClientRect();
        const diffWidth = rect.width - this.diameter;
        const randomX = []
        this.balls.forEach((_, index) => {
          randomX[index] = Math.round(Math.random() * diffWidth)
        })
        this.matchedBalls.forEach((value, index) => {
          randomX[value - 1] = (rect.width / 4) * index
        })
        this.randomXArray = randomX;
      }
    }
  },
  methods: {
    toString(value) {
      let s = String(value)
      if (s.length === 1) {
        s = '0' + s;
      }
      return s;
    },
    ballClass(value) {
      if (!this.running) return ''
      const index = this.matchedBalls.indexOf(value)
      if (index !== -1) {
        return index !== this.matchedBalls.length - 1 ? 'bounce yellow' : 'bounce red'
      } else {
        return 'fall'
      }
    },
    animationend() {
      this.$emit('updateTrigger', false)
    },
    setDiameter() {
      const rect = this.$refs['ball-pool'].getBoundingClientRect();
      const width = Math.round(rect.width / 4);
      const height = Math.round(rect.height / 4);
      if (width > height) {
        this.diameter = height - 5;
      } else {
        this.diameter = width - 5;
      }
      const diffWidth = rect.width - this.diameter;
      const randomX = []
      this.balls.forEach((_, index) => {
        randomX[index] = Math.round(Math.random() * diffWidth)
      })
      this.randomXArray = randomX;
    }
  }
}
</script>

<style lang="scss" scoped>
#ball-pool {
  position: relative;
  height: 100%;
  margin: 0 20px;
  .ball {
    border-radius: 50%;
    position: absolute;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #c3c3c3;
    box-shadow: inset -5px -15px 30px rgba(0,0,0,.5);
    background-image: linear-gradient(-45deg, rgba(255,255,220,.3) 0%, transparent 100%);
    &.fall {
      animation: fall .5s cubic-bezier(.63,.09,.75,.46) forwards;
    }
    &.bounce {
      animation: bounce 1s cubic-bezier(.63,.09,.75,.46) forwards;
      &.yellow {
        background-color: #fff2ca;
      }
      &.red {
        background-color: #e99695;
      }
    }
    @keyframes fall {
      0% {
        transform: translateY(0%);
      }
      100% {
        transform: translateY(442%);
      }
    }
    @keyframes bounce {
      0% {
        transform: translateY(0%);
      }
      50% {
        transform: translateY(442%);
      }
      100% {
        transform: translateY(100%);
      }
    }
    .text {
      opacity: 0.01;
      &.text-animation {
        animation: opacity-animation 1s 1s forwards;
        @keyframes opacity-animation {
          100% {
            opacity: 1;
          }
        }
      }
    }
  }
}
</style>

