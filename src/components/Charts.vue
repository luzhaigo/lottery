<template>
<div id="charts">
  <div class="th-ball-area">
    <div class="w-50">
      <h2>The First Ball</h2>
      <times-chart :ballCounts="firstBallCounts"></times-chart>
    </div>
    <div class="w-50">
      <h2>The Second Ball</h2>
      <times-chart :ballCounts="secondBallCounts"></times-chart>
    </div>
    <div class="w-50">
      <h2>The Third Ball</h2>
      <times-chart :ballCounts="thirdBallCounts"></times-chart>
    </div>
    <div class="w-50">
      <h2>The Forth Ball</h2>
      <times-chart :ballCounts="forthallCounts"></times-chart>
    </div>
  </div>
  <div class="other-area">
    <div class="w-50">
      <h2>Draw History</h2>
      <number-chart :accNumbers="accNumbers"></number-chart>
    </div>
    <div class="w-50">
      <h2>Odd/Even</h2>
      <odd-even-chart :oddEven="oddEven"></odd-even-chart>
    </div>
  </div>
</div>
</template>

<script>
import NumberChart from './NumberChart.vue';
import TimesChart from './TimesChart.vue';
import OddEvenChart from './OddEvenChart.vue';

export default {
  props: {
    drawHistory: {
      type: Array,
      default: () => [],
    }
  },
  components: {
    NumberChart,
    TimesChart,
    OddEvenChart,
  },
  data() {
    return {
      accNumbers: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      firstBallCounts: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      secondBallCounts: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      thirdBallCounts: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      forthallCounts: [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
      oddEven: [0, 0],
    }
  },
  watch: {
    'drawHistory.length': function(newV, oldV) {
      if (newV > oldV) {
        const latestDraw = this.drawHistory[0];
        latestDraw.forEach((value, index) => {
          this.$set(this.accNumbers, value-1, this.accNumbers[value-1] + 1)
          this.$set(this.oddEven, value % 2, this.oddEven[value % 2] + 1)
          switch (index) {
            case 0: {
              this.$set(this.firstBallCounts, value-1, this.firstBallCounts[value-1] + 1)
              break;
            }
            case 1: {
              this.$set(this.secondBallCounts, value-1, this.secondBallCounts[value-1] + 1)
              break;
            }
            case 2: {
              this.$set(this.thirdBallCounts, value-1, this.thirdBallCounts[value-1] + 1)
              break;
            }
            case 3:{
              this.$set(this.forthallCounts, value-1, this.forthallCounts[value-1] + 1)
              break;
            }
            default:
              break;
          }
        })
      }
    }
  },
}
</script>

<style lang="scss" scoped>
#charts {
  .th-ball-area, .other-area {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
    .w-50 {
      width: 40%;
    }
  }
}
</style>
