<template>
  <div id="app">
    <Training/>
  </div>
</template>

<script>
import Training from './components/Training.vue'

export default {
  name: 'App',
  components: {
    Training
  }
}
</script>

<style>
#app {
  font-family: Courier, monospace;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  text-align: center;
  flex-direction: column;
  align-items: center;
  color: #2c3e50;
  font-size: 8vmin;
}

</style>
