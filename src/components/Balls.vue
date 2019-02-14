<template>
<div class="row" ref="row">
  <div v-for="value in balls" :key="value" :class="['ball', ballClass(value)]" :style="{
      width: `${diameter}px`,
      height: `${diameter}px`,
    }">
      {{toString(value)}}
  </div>
</div>
</template>

<script>
export default {
  props: {
    balls: {
      type: Array,
    },
    matchedBalls: {
      type: Array,
      default: () => [],
    },
    animationValue: {
      type: Array,
      default: () => [],
    },
    running: {
      type: Boolean,
      default: false,
    }
  },
  data() {
    return {
      diameter: 0,
    }
  },
  mounted() {
    this.$nextTick(this.setDiameter);
    this.resize = window.addEventListener('resize', this.setDiameter)
  },
  updated() {
    this.$nextTick(this.setDiameter);
  },
  beforeDestroy() {
    window.removeEventListener(this.resize)
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
      if (this.animationValue.includes(value)) {
        return 'animation'
      }
      if (!this.running) {
        const index = this.matchedBalls.indexOf(value);
        if (index !== -1) {
          return index < 3 ? 'normal' : 'special';
        }
      }
    },
    setDiameter() {
      const rect = this.$refs.row.getBoundingClientRect();
      const width = Math.round(rect.width / 4);
      const height = rect.height
      if (width > height) {
        this.diameter = height - 5;
      } else {
        this.diameter = width - 5;
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  .ball {
    border-radius: 50%;
    border: 1px solid black;
    display: flex;
    justify-content: center;
    align-items: center;
    &.animation {
      background-color: blue;
      color: white;
    }
    &.normal {
      background-color: #fff2ca;
    }
    &.special {
      background-color: #e99695;
    }
  }
}
</style>

