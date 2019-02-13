<template>
  <div id="lottery">
    <div class="panel left">
      <div class="number-panel left">
        <balls 
          v-for="(balls, index) in ballsArray" 
          :balls="balls" 
          :key="index" 
          class="flex" 
          :matchedBalls="drawResult" 
          :animationValue="animationValue"
          :running="running"></balls>
      </div>
      <button id="draw-button" @click="startDraw">Draw</button>
    </div>
    <div class="panel right">
      <div class="latest-draw">
        <balls v-if="ballsHistory[0]" :balls="ballsHistory[0]" class="flex" :matchedBalls="drawResult"></balls>
      </div>
      <div class="number-panel right">
        <template v-if="ballsHistory.length">
          <balls v-for="(balls, index) in fourDraws" :balls="balls" :key="index" class="flex"></balls>
        </template>
      </div>
    </div>
  </div>
</template>

<script>
import Balls from './Balls.vue';
export default {
  components: {
    Balls,
  },
  data() {
    return {
      ballsArray: [[1,2,3,4], [5, 6, 7, 8], [9, 10, 11, 12],[13, 14, 15, 16]],
      drawResult: [],
      ballsHistory: [],
      running: false,
      animationValue: [],
    }
  },
  computed: {
    fourDraws() {
      const arr = this.ballsHistory.slice(1, 5)
      for (let i = 4 - arr.length;i--;i>0) {
        arr.push([])
      }
      return arr;
    }
  },
  watch: {
    running(newV, oldV) {
      if(newV && !oldV) {
        this.timerID = setInterval(() => {
          this.animationValue = this.generateDraw();
        },100)
      } else {
        clearInterval(this.timerID);
        this.animationValue = [];
      }
    },
  },
  methods: {
    startDraw() {
      if (this.running) return;
      setTimeout(() => {
        const drawResult = this.generateDraw();
        this.running = false;
        this.drawResult = drawResult;
        this.ballsHistory.unshift(drawResult)
      }, 3000);
      this.running = true;
    },
    generateDraw() {
      const drawResult = [];
      for(;;) {
        const ball = Math.floor(Math.random() * 16) + 1
        if (!drawResult.includes(ball))
          drawResult.push(ball)
        if (drawResult.length === 4)
          break;
      }
      return drawResult
    }
  }
}
</script>

<style lang="scss" scoped>
#lottery {
  width: 50vw;
  height: 50vh;
  border: solid 1px black;
  border-radius: 10px;
  margin: auto;
  display: flex;
  .panel {
    width: 50%;
    padding: 20px 0 20px;
    display: flex;
    flex-flow: column;
    &.left {
      padding-left: 20px;
      padding-right: 10px;
    }
    &.right {
      padding-left: 10px;
      padding-right: 20px;
    }
  }
  .latest-draw {
    flex: 1;
    border: 1px solid black;
    border-radius: 5px;
    display: flex;
    flex-flow: column;
    padding: 10px 20px;
  }
  .number-panel {
    box-sizing: border-box;
    padding: 20px;
    border: 1px solid black;
    height: 75%;
    border-radius: 5px;
    display: flex;
    flex-flow: column;
    &.left {
      margin-bottom: 20px;
    }
    &.right {
      margin-top: 5px;
    }
  }
}
.flex {
  flex: 1;
}
#draw-button {
  width: 100%;
  flex: 1;
  padding: 0;
  border-radius: 5px;
  border: 1px solid black;
  font-size: 16px;
}
</style>
