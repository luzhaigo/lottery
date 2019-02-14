<template>
<vue-c3 :handler="handler"></vue-c3>
</template>

<script>
import Vue from 'vue'
import VueC3 from 'vue-c3'

export default {
  props: {
    oddEven: {
      type: Array,
    }
  },
  components: {
    VueC3,
  },
  data() {
    return {
      handler: new Vue(),
    }
  },
  computed: {
    data() {
      return  {
        columns: [
          ['even'].concat(this.oddEven[0]),
          ['odd'].concat(this.oddEven[1]),
        ],
        type: 'pie',
      }
    },
    options() {
      const options = {
        data: this.data,
        legend: {
          item: {
            onclick: () => {}
          }
        },
        size: {
          height: 200
        }
      }
      return options
    },
  },
  watch: {
    oddEven() {
      this.handler.$emit('dispatch', (chart) => {
        chart.load(this.data)
      });
    }
  },
  mounted() {
    this.handler.$emit('init', this.options);
  },
}
</script>

<style lang="scss" scoped>
</style>