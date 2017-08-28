<template>
<div class="contenedor">
  <ul class="nav nav-tabs">
    <li class="active"><a data-toggle="tab" href="#home">Home</a></li>
    <li><a data-toggle="tab" href="#menu1">Entradas</a></li>
    <li><a data-toggle="tab" href="#menu2">Peliculas</a></li>
  </ul>

  <div class="tab-content">
    <div id="home" class="tab-pane fade in active classCentrado">
      <h3>HOME</h3>
      <p>Some content.</p>
    </div>
    <entrada id="menu1" class="tab-pane fade">
    </entrada>
    <pelicula id="menu2" class="tab-pane fade">
    </pelicula>
  </div>
</div>
<!--
  class="tab-pane fade in active" => Para que sea un panel y por defecto activo

  <div id="contenedor">
    <entrada></entrada>
    <pelicula></pelicula>
  </div>
 --> 
</template>

<script>
import entrada from './Entrada.vue'
import pelicula from './Pelicula.vue'
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
    entrada, pelicula
  }
}
</script>

<style>
.classCentrado {
    text-align: center;
}

</style>
