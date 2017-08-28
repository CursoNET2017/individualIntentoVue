<template>
  <div id="app">
    <contenedor></contenedor>
  </div>
</template>

<script>
import contenedor from './components/Contenedor.vue'

export default {
  name: 'app',
  components: {
    contenedor
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
