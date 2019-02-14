<template>
<vue-c3 :handler="handler"></vue-c3>
</template>

<script>
import Vue from 'vue'
import VueC3 from 'vue-c3'

export default {
  props: {
    accNumbers: {
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
          [' '].concat(this.accNumbers),
        ],
        type: 'bar',
      }
    },
    options() {
      const options = {
        data: this.data,
        tooltip: {
          format: {
            title: function (x, index) { return 'No.' + (index + 1); },
            value: function (value) {
              return value += value > 1 ? ' times' : ' time';
            }
          }
        },
        axis: {
          x: {
              type: 'category',
              label: {
                text: 'No. of ball',
                position: 'outer-right',
              },
              height: 50,
              categories: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16]
          },
          y: {
            min: 0,
            label: {
              text: 'Times',
              position: 'outer-top',
            },
            tick: {
                format: function (d) {
                    return (parseInt(d) == d) ? d : null;
                }
            },
            padding: {bottom: 0}
          }
        },
        legend: {
          show: false
        },
        size: {
          height: 200
        }
      }
      return options
    },
  },
  watch: {
    accNumbers: {
      handler() {
        this.handler.$emit('dispatch', (chart) => {
          chart.load(this.data)
        });
      },
      deep: true,
    }
  },
  mounted() {
    this.handler.$emit('init', this.options);
  },
}
</script>

<style lang="scss" scoped>
</style>