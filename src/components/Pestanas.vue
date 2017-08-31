<template>
    <!--<div class="pestanas">
        <div class="btn-group btn-group-lg" role="group" aria-label="Basic example">
            <button type="button" class="btn btn-primary btn-lg" v-on:click="cambiarContenido(0)">Inicio</button>
            <button type="button" class="btn btn-primary btn-lg" v-on:click="cambiarContenido(1)">Entradas</button>
            <button type="button" class="btn btn-primary btn-lg" v-on:click="cambiarContenido(2)">Peliculas</button>            
        </div>
    </div>    --> 
    <nav class="navbar navbar-dark bg-primary">
        <div class="container-fluid">
        <div class="navbar-header">
          <a class="navbar-brand" href="#">Web_ULALA</a>
        </div>
        <ul class="nav navbar-nav">
            <li class="active"><a v-on:click="cambiarContenido(0)">Home</a></li>
            <li><a v-on:click="cambiarContenido(1)">Entradas</a></li>
            <li><a v-on:click="cambiarContenido(2)">Peliculas</a></li>
        </ul>
        </div>
    </nav> 
</template>

<script>
export default {
  name: 'pestanas',
  //props: ['ventana'],//props en los hijos. En este caso Pestañas no lo usa para nada
  data() {
    return {      
    }    
   },
    methods: {
        cambiarContenido: function(ven){
            //this.ventana = ven;
            this.$emit('envioVentana', ven)
            //alert('emitido '+ ven);
        }
    }
}
</script>

<style>
    .pestanas {
        text-align: center;
    }
    a {
        color: white;
    }
</style>
