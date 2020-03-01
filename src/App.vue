<template>
  <div id="app">
    
    <mainBar/>
  </div>
</template>

<script>
import mainBar from './components/mainBar.vue'

export default {
  name: 'App',
  components: {
    mainBar
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
