<template>
  <div id="app">
    <pestanas v-on:envioVentana="modificarVentana"></pestanas>
    <contenedor v-bind:datospadre="ventana"></contenedor>
  </div>
</template>

<script>
//import contenedor from './components/Contenedor.vue'
import contenedor from './components/Contenedor2.vue'
import pestanas from './components/Pestanas.vue'

export default {
  name: 'app',
  data() {
    return{//Aqui la mierda de datos si tengo que pasar mas datos
      ventana: 0
    };
    
  },
  methods: {
    modificarVentana: function(ven) {
      this.ventana = ven;
    }
  },
  components: {
    contenedor, pestanas
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
