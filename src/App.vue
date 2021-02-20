<template>
  <div id="app">
    <Perguntas/>
  </div>
</template>

<script>
import Perguntas from './components/Perguntas.vue'

export default {
  name: 'App',
  components: {
    Perguntas
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
