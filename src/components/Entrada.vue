<template>
  <div id="entrada">
    <h1>Entradas</h1>
    <ul>
        <li v-for="item in items" v-bind:key="item.Id" v-on:click="cargarClikMaestro(item.Id)">
           {{item.Titulo}}
        </li>
    </ul>
    <button v-on:click="visibleBorrado">Nueva</button>
    <form id= "detalle" v-show="visible" v-on:submit.prevent="onSubmit">
        {{item}}
         <div class="linea">
            <label>Titulo: </label>
            <input name="titulo" type="text" placeholder="Introduce Título" v-model="item.titulo">
        </div>
        <div class="linea">
            <label>Fila: </label>
            <input name="fila" type="number" placeholder="Introduce Fila" v-model="item.fila">
        </div>
        <div class="linea">
            <label>Butaca: </label>
            <input name="butaca" type="number" placeholder="Introduce Butaca" v-model="item.butaca">
        </div>        
        <div id="botones">
            <button id="buttonCrear" v-on:click="crear">Añadir</button>
            <button id="buttonActualizar" v-on:click="actualizar">Actualizar</button>
            <button id="buttonBorrar" v-on:click="borrar">Borrar</button>
        </div>
    </form>

  </div>
</template>

<script>


export default {
  name: 'contenedor',
  data() {      
    return {
        items: [],
        visible : false,
        item:{id: '', titulo: '', butaca: '',fila: ''}
    };
  },
  methods: {
    getTodos: function () {
        let _this = this;
        $.ajax({
            type: 'GET',
            url: 'http://localhost:50422/api/Entradas/',
            success: function (response) {
                _this.items = JSON.parse(JSON.stringify(response));
            },
            //Si hay error
            error : function(){
                alert('Problemas al cargar el listado');
                debugger;
            }
        });
    },
    visibleBorrado: function() {
        //this.visible = !this.visible;
        this.visible = true;
        this.item.id = '';
        this.item.titulo = '';
        this.item.butaca = '';
        this.item.fila = '';
    },
    cargarClikMaestro: function(refer){
        let _this = this;
        $.ajax({
            url: 'http://localhost:50422/api/Entradas/'+refer,
            type : 'GET',     
            success : function(data) {
            //alert('YUPII');
            _this.rellenarDatosClick(data);
            },
            error : function(){
            debugger;
            }
        });
    },
    rellenarDatosClick: function(data) {
        this.visible = true;
        this.item.id = data.Id;
        this.item.titulo = data.Titulo;
        this.item.butaca = data.Butaca;
        this.item.fila = data.Fila;
    },
    onSubmit: function(event){
        this.getTodos();
    },
    crear: function(event){
        let _this = this;
        var titulo1 = this.item.titulo;
        var butaca1 = this.item.butaca;
        var fila1 = this.item.fila;
        $.ajax({
            type: 'POST',
            url: 'http://localhost:50422/api/Entradas/',
            data: {Titulo: titulo1, Butaca: butaca1, Fila: fila1},
            success: function (data) {
                _this.getTodos();
                _this.visibleBorrado();
            },
            //Si hay error
            error : function(){
                alert('Problemas al insertar el item');
                debugger;
            }
        });
    },
    actualizar: function(){

    },
    borrar: function(){

    }


  },
  created() {
      this.getTodos();
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
li {
    list-style-type: none;
    width:100%;
}

</style>
