<template>
<div class="row" ref="row">
  <div class="ball" v-for="value in balls" :key="value" :style="{
      width: `${diameter}px`,
      height: `${diameter}px`,
      'background-color': color(value),
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
  },
  data() {
    return {
      diameter: 0,
    }
  },
  mounted() {
    this.$nextTick(() => {
      const rect = this.$refs.row.getBoundingClientRect();
      const width = Math.round(rect.width / 4);
      const height = rect.height
      if (width > height) {
        this.diameter = height - 5;
      } else {
        this.diameter = width - 5;
      }
    });
  },
  methods: {
    toString(value) {
      let s = String(value)
      if (s.length === 1) {
        s = '0' + s;
      }
      return s;
    },
    color(value) {
      const index = this.matchedBalls.indexOf(value);
      if (index !== -1) {
        return index < 3 ? 'yellow' : 'red';
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
  }
}
</style>

