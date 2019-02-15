<template>
  <div id="app">
    <h1>Lottery Game</h1>
    <div class="radios">
      <input type="radio" :value="0" v-model="modelType" :disabled="disabled"/>Mode1
      <div class="space"></div>
      <input type="radio" :value="1" v-model="modelType" :disabled="disabled"/>Mode2
    </div>
    <lottery :drawHistory.sync="drawHistory" :modelType="modelType" @update:disabled="disabled = $event"></lottery>
    <h1>Charts</h1>
    <charts :drawHistory="drawHistory"></charts>
  </div>
</template>

<script>
import Lottery from './components/Lottery.vue'
import Charts from './components/Charts.vue'

export default {
  name: 'app',
  components: {
    Lottery,
    Charts,
  },
  data() {
    return {
      drawHistory: [],
      modelType: 0,
      disabled: false,
    }
  },
  methods: {
    updateDrawHistory(drawHistory) {
      this.drawHistory = drawHistory;
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  .radios {
    width: 50vw;
    margin: auto;
    display: flex;
    .space {
      width: 10px;
    }
  }
}
</style>
