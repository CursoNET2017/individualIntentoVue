<template>
  <div id="entrada">
    <div class="classCentrado">
        <h1>Entradas</h1>
        <ul class="list-group">
            <!--<li class="list-group-item" v-for="item in items" v-bind:key="item.Id" v-on:click="cargarClikMaestro(item.Id)">
            {{item.Titulo}}
            </li>-->
            <a class="list-group-item" v-for="item in items" v-bind:key="item.Id" v-on:click="cargarClikMaestro(item.Id)">
            {{item.Titulo}}
            </a>
        </ul>
        <button v-on:click="visibleBorrado" class="btn btn-info btn-responsive btninter">Nueva</button>
    </div>
    <div class="container">
    <form id= "detalle" class="form-horizontal" v-show="visible" v-on:submit.prevent="onSubmit">
        {{item}}: {{valido}}
        <!-- <div class="linea">
            <label>Titulo: </label>
            <input name="titulo" type="text" placeholder="Introduce Título" v-model="item.titulo" v-on:input="validar" class="form-control" v-bind:class="{claseError: !valido.a}">
        </div> -->
         <div class="form-group">
            <label class="col-sm-2 control-label">Titulo: </label>
            <div class="col-sm-10">
                <input name="titulo" type="text" placeholder="Introduce Título" v-model="item.titulo" v-on:input="validar" class="form-control" v-bind:class="{claseError: !valido.a}">
            </div>
            <div class="ayudaInput"><small v-if="!valido.a" id="passwordHelpBlock" class="form-text text-danger"> Entre 0 y 30 caracteres.</small></div>
        </div>
         <div class="ayudaInput"></div>
        <div class="form-group">
            <label class="col-sm-2 control-label">Fila: </label>
            <div class="col-sm-10">
                <input name="fila" type="number" placeholder="Introduce Fila" v-model="item.fila" v-on:input="validar" class="form-control" v-bind:class="{claseError: !valido.b}">
            </div>
            <div class="ayudaInput"><small v-if="!valido.b" id="passwordHelpBlock" class="form-text text-danger"> Número entero positivo.</small></div>
        </div>
        <div class="ayudaInput"></div>
        <div class="form-group">
            <label class="col-sm-2 control-label">Butaca: </label>
            <div class="col-sm-10">
                <input name="butaca" type="number" placeholder="Introduce Butaca" v-model="item.butaca" v-on:input="validar" class="form-control" v-bind:class="{claseError: !valido.c}">
            </div>
            <div class="ayudaInput"><small v-if="!valido.c" id="passwordHelpBlock" class="form-text text-danger"> Número entero positivo.</small></div>
        </div>   
        <div class="ayudaInput"></div>     
        <div id="botones" class="classCentrado">
            <button id="buttonCrear" v-on:click="crear" v-bind:disabled="editable || !valido.a || !valido.b || !valido.c" class="btn btn-info btn-responsive btninter">Añadir</button>
            <button id="buttonActualizar" v-on:click="actualizar" v-bind:disabled="!item.id || !valido.a || !valido.b || !valido.c" class="btn btn-info btn-responsive btninter">Actualizar</button>
            <button id="buttonBorrar" v-on:click="borrar" v-bind:disabled="!item.id" class="btn btn-info btn-responsive btninter">Borrar</button>
        </div>
    </form>
    </div>
  </div>
</template>

<script>


export default {
  name: 'entrada',
  data() {      
    return {
        items: [],
        visible : false,
        editable: false,
        valido: {a: false, b: false, c: false},
        item:{id: '', titulo: '', butaca: '',fila: ''}
    };
  },
  methods: {
    validar: function(){
        let _valido = {a: true, b: true, c: true};
        if(!(this.item.titulo.length > 0 && this.item.titulo.length <=30)){
            _valido.a = false;
            this.erroneo = true;
        };
        if(!(this.item.fila > 0 && this.item.fila % 1 == 0)){
            _valido.b = false;
        };
        if(!(this.item.butaca > 0 && this.item.butaca % 1 == 0)){
            _valido.c = false;
        };
        this.valido = _valido;
    },
    getTodos: function () {
        let _this = this;
        $.ajax({
            type: 'GET',
            url: 'http://localhost:50422/api/Entradas/',
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
        this.valido = {a: false, b: false, c: false};//Volvemos a dejar las validaciones a falso
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
        this.valido = {a: true, b: true, c: true};//Volvemos a dejar las validaciones a falso
        this.item.id = response.Id;
        this.item.titulo = response.Titulo;
        this.item.butaca = response.Butaca;
        this.item.fila = response.Fila;
    },
    onSubmit: function(event){
        //alert('onSubmit');
        this.getTodos();
    },
    crear: function(event){
        let _this = this;
        let titulo1 = this.item.titulo;
        let butaca1 = this.item.butaca;
        let fila1 = this.item.fila;
        $.ajax({
            type: 'POST',
            url: 'http://localhost:50422/api/Entradas/',
            data: {Titulo: titulo1, Butaca: butaca1, Fila: fila1},
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
		    url : 'http://localhost:50422/api/Entradas/'+id1,
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
            url: 'http://localhost:50422/api/Entradas/'+id1,
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
.classCentrado {
    text-align: center;
}
.col-sm-2 control-label {
    text-align: left;
}
li {
    list-style-type: none;
}
.claseError {
    border-color: #a94442;
}
.ayudaInput{
    height: 20px;
    margin-left: 20%;
}

</style>