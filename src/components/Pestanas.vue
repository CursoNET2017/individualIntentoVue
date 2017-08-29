<template>
    <div class="pestanas">
        <div class="btn-group btn-group-lg" role="group" aria-label="Basic example">
            <button type="button" class="btn btn-primary btn-lg" v-on:click="cambiarContenido(0)">Inicio</button>
            <button type="button" class="btn btn-primary btn-lg" v-on:click="cambiarContenido(1)">Entradas</button>
            <button type="button" class="btn btn-primary btn-lg" v-on:click="cambiarContenido(2)">Peliculas</button>            
        </div>
    </div>  
</template>

<script>
export default {
  name: 'pestanas',
  props: ['ventana'],//props en los hijos
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
</style>
