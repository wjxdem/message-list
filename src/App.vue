<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <messagelist/>
  </div>
</template>

<script>
import messagelist from './messageList/index.vue'

export default {
  name: 'App',
  components: {
    messagelist
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
