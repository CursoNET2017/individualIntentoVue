<template>
  <div id="contenedor">
    <entrada></entrada>

  </div>
</template>

<script>
import entrada from './Entrada.vue'
//import detallePersona from './components/DetallePersona.vue'


export default {
  name: 'app',
  data() {
    return {
      datos: {
        
      }
    }
  },
  components: {
    entrada
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

</style>
