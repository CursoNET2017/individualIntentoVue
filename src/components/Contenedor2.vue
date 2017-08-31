<template>
    <div class="contenedor">
        <div v-show="datospadre == 0">
            <h2>¡Bienvenido!</h2>
        </div>
        <div v-show="datospadre == 1">
            <entrada></entrada>
        </div>
        <div v-show="datospadre == 2">
            <pelicula></pelicula>
        </div>
    </div>  
</template>

<script>
import entrada from './Entrada.vue'
import pelicula from './Pelicula.vue'


export default {
  name: 'contenedor2',
  props: ['datospadre'],//props en los hijos
  data() {
    return {

    }
  },
  components: {
    entrada, pelicula
  }
}
</script>

<style>
.classCentrado {
    text-align: center;
}
h2 {
    text-align: center;
    margin-top: 60px;
}

</style>
