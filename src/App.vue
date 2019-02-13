<template>
  <div id="app">
    <lottery></lottery>
  </div>
</template>

<script>
import Lottery from './components/Lottery.vue'

export default {
  name: 'app',
  components: {
    Lottery,
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
