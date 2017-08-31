<template>
  <div id="pelicula">
        <div class="classCentrado">
            <h1>Peliculas</h1>
            <ul class="list-group">
                <a class="list-group-item" v-for="item in items" v-bind:key="item.Id" v-on:click="cargarClikMaestro(item.Id)">
                    {{item.Titulo}}
                </a>
            </ul>
            <button class="btn btn-success" v-on:click="visibleBorrado">Nueva</button>
        </div>
    <form id= "detalle" v-show="visible" v-on:submit.prevent="onSubmit">
        {{item}}: {{valido}}
         <div class="input-group">
            <span class="input-group-addon">Titulo:</span>
            <input name="titulo" type="text" placeholder="Introduce Título" v-model="item.titulo" v-on:input="validar" v-bind:class="{claseError: !valido.a}" class="form-control">            
            <!--<span v-if="!valido.a" class="help-block">Entre 0 y 30 caracteres.</span> -->
        </div>
        <div class="ayudaInput"><small v-if="!valido.a" id="passwordHelpBlock" class="form-text text-danger"> Entre 0 y 30 caracteres.</small></div>
        <div class="input-group">
            <span class="input-group-addon">Director:</span>
            <input name="director" type="text" placeholder="Introduce Director" v-model="item.director" v-on:input="validar" v-bind:class="{claseError: !valido.b}" class="form-control">
        </div>
        <div class="ayudaInput"><small v-if="!valido.b" id="passwordHelpBlock" class="form-text text-danger"> Entre 0 y 30 caracteres.</small></div>
        <div class="input-group">
            <span class="input-group-addon">Duración:</span>
            <input name="duracion" type="number" placeholder="Introduce Duración" v-model="item.duracion" v-on:input="validar" v-bind:class="{claseError: !valido.c}" class="form-control">
        </div>    
        <div class="ayudaInput"><small v-if="!valido.c" id="passwordHelpBlock" class="form-text text-danger"> Duración en minutos. Sin decimales.</small></div>
        <div class="input-group">
            <span class="input-group-addon">País:</span>
            <input name="pais" type="text" placeholder="Introduce País" v-model="item.pais" v-on:input="validar" v-bind:class="{claseError: !valido.d}" class="form-control">
        </div>        
        <div class="ayudaInput"><small v-if="!valido.d" id="passwordHelpBlock" class="form-text text-danger"> Entre 0 y 30 caracteres.</small></div>
        
        <div id="botones" class="classCentrado">
            <button id="buttonCrear" class="btn btn-success" v-on:click="crear" v-bind:disabled="editable || !valido.a || !valido.b || !valido.c || !valido.d">Añadir</button>
            <button id="buttonActualizar" class="btn btn-success" v-on:click="actualizar" v-bind:disabled="!item.id || !valido.a || !valido.b || !valido.c || !valido.d">Actualizar</button>
            <button id="buttonBorrar" class="btn btn-success" v-on:click="borrar" v-bind:disabled="!item.id">Borrar</button>
        </div>
    <!--
        <div  class="btn-group btn-group-justified">
            <div class="btn-group">
                <button id="buttonCrear" class="btn btn-success" v-on:click="crear" v-bind:disabled="editable || !valido.a || !valido.b || !valido.c || !valido.d">Añadir</button>
            </div>
            <div class="btn-group">
                <button id="buttonActualizar" class="btn btn-success" v-on:click="actualizar" v-bind:disabled="!item.id || !valido.a || !valido.b || !valido.c || !valido.d">Actualizar</button>
            </div>
            <div class="btn-group">
                <button id="buttonBorrar" class="btn btn-success" v-on:click="borrar" v-bind:disabled="!item.id">Borrar</button>
            </div>            
        </div>
    -->
    </form>

  </div>
</template>

<script>
export default {
  name: 'peliculas',
  data() {      
    return {
        items: [],
        visible : false,
        editable: false,
        valido: {a: false, b: false, c: false, d: false},
        item:{id: '', titulo: '', director: '', duracion: '', pais: ''}
    };
  },
  methods: {
    validar: function(){
        let _valido = {a: true, b: true, c: true, d: true};
        if(!(this.item.titulo.length > 0 && this.item.titulo.length <=30)){
            _valido.a = false;
            this.erroneo = true;
        };
        if(!(this.item.director.length > 0 && this.item.director.length <=30)){
            _valido.b = false;
        };
        if(!(this.item.duracion > 0 && this.item.duracion % 1 == 0)){
            _valido.c = false;
        };
        if(!(this.item.pais.length > 0 && this.item.pais.length <=30)){
            _valido.d = false;
        };
        this.valido = _valido;
    },
    getTodos: function () {
        let _this = this;
        $.ajax({
            type: 'GET',
            url: 'http://localhost:50422/api/Peliculas/',
            success: function (response) {
                _this.items = JSON.parse(JSON.stringify(response));
                //_thhis.items = response;
            },
            error : function(){
                alert('Problemas al cargar el listado');
                debugger;
            }
        });
    },
    visibleBorrado: function() {
        //this.visible = !this.visible;
        this.visible = true;
        this.editable =  false;//No estamos editando
        this.valido = {a: false, b: false, c: false, d: false};//Volvemos a dejar las validaciones a falso
        this.item.id = '';
        this.item.titulo = '';
        this.item.director = '';
        this.item.duracion = '';
        this.item.pais = '';
    },
    cargarClikMaestro: function(refer){
        let _this = this;
        $.ajax({
            url: 'http://localhost:50422/api/Peliculas/'+refer,
            type : 'GET',     
            success : function(response) {
            //alert('YUPII');
            _this.editable = true;
            _this.rellenarDatosClick(response);
            },
            error : function(){
            debugger;
            }
        });
    },
    rellenarDatosClick: function(response) {
        this.visible = true;
        this.valido = {a: true, b: true, c: true, d: true};//Volvemos a dejar las validaciones a falso
        this.item.id = response.Id;
        this.item.titulo = response.Titulo;
        this.item.director = response.Director;
        this.item.duracion = response.Duracion;
        this.item.pais = response.Pais;
    },
    onSubmit: function(event){
        //alert('onSubmit');
        this.getTodos();
    },
    crear: function(event){
        let _this = this;
        let titulo1 = this.item.titulo;
        let director1 = this.item.director;
        let duracion1 = this.item.duracion;
        let pais1 = this.item.pais;
        $.ajax({
            type: 'POST',
            url: 'http://localhost:50422/api/Peliculas/',
            data: {Titulo: titulo1, Director: director1, Duracion: duracion1, Pais: pais1},
            success: function (response) {
                //_this.getTodos();
                _this.visibleBorrado();
            },
            //Si hay error
            error : function(){
                alert('Problemas al insertar el item');
                debugger;
            },        
		    complete : function(xhr, status) {
		        //alert('Creado '+titulo1+' con exito');
		        _this.getTodos();
		    }
        });
    },
    actualizar: function(){
        var id1 = this.item.id;
    	//var nombre1 = this.item.nombre;
	  	//var apellidos1 = this.item.apellidos;
	  	//var edad1 = this.item.edad;
	  	//alert(nombre1+' '+apellidos1);
        let _this = this;
	  	$.ajax({
		    url : 'http://localhost:50422/api/Peliculas/'+id1,
		    type : 'PUT',     
		    dataType : 'json',
		    //data: {Id : id1, Nombre : nombre1, Apellidos: apellidos1, Edad: edad1 },
            data: _this.item,
		    success : function(response) {            
		        _this.visibleBorrado();
		    },
		    error : function(){
		    	//alert('CACA');
		     	debugger;
		    },        
		    complete : function(xhr, status) {
		        //alert('Actualización con exito');
		        _this.getTodos();
		    }
	  	});
    },
    borrar: function(){
        let id1 = this.item.id;
        let titulo1 = this.item.titulo;
        let _this = this;
        $.ajax({
            type: 'DELETE',
            url: 'http://localhost:50422/api/Peliculas/'+id1,
            success: function () {
                //_this.getTodos();
                _this.visibleBorrado();
            },
            error : function(){
                alert('Problemas al borrar el item');
                debugger;
            },
            complete : function() {
                _this.getTodos();
                //alert('Eliminado con exito '+titulo1);
            }
        })
    }    
  },
  created() {
      this.getTodos();
  }
}
</script>

<style>

li {
    list-style-type: none;
}
.claseError {
    border-color: #a94442;
}
.classCentrado {
    text-align: center;
}
#botones{
    margin-left: 10%;
    margin-right: 10%;
}
/*
.input-group{
    margin-left: 5%;
    margin-right: 5%;
}
*/
.input-group{
    width: 60%;
    margin-left: 20%;
}
.ayudaInput{
    height: 20px;
    margin-left: 20%;    
}
</style>