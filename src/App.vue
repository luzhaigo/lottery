<template>
  <div id="app">
    <h1>Lottery Game</h1>
    <lottery :drawHistory.sync="drawHistory"></lottery>
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
    }
  },
  methods: {
    updateDrawHistory(drawHistory) {
      this.drawHistory = drawHistory;
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
