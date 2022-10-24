<template>
    <update v-bind:update="false" form-title="Registrar"></update>

    <h5>Buscador de peliculas {{textoABuscar}}</h5>
        <form action="">
        <div class="input-group mb-3">
            <input type="text" v-model="textoABuscar" class="form-control" placeholder="Filtrar menor precio" >
            <button class="btn btn-outline-secondary" @click.prevent="getMenorPrecio(textoABuscar)">Filtrar</button>

        </div>
   
       </form>
   
    <h1>Listado de pelicula</h1>

    <input type="text" v-model="buscar" class="form-control" placeholder="Escribe para buscar una pelicula....."/>
    
    <div class="row mt-3">
    
        <div class="col-md-4" v-for="item in items" v-bind:key="item.id"> 

            <div class="card mb-3">
                <!--<img class="card-img-top" v-bind:src="'img/' + item.img" v-bind:alt="item.nombre">-->
                <div class="card-body">
                <h3 class="card-title mb-3"><strong>Nombre:</strong>{{ item.nombre }}</h3>
                <p class="card-text"><strong>Descripcion:</strong> {{ item.descripcion }}</p>
                <p class="card-text"><strong>Imagen:</strong> {{ item.imagen }}</p>
                <p class="card-text"><strong>Precio:</strong> {{ item.precio }}</p>
                <p class="card-text"><strong>Categorias:</strong> {{ item.categorias }}</p>
                <p class="card-text"><strong>Horarios:</strong> {{ item.horarios }}</p>
		<div><button @click="removePelicula(item.id)">eliminar {{item.id}}</button></div>
                <button v-on:click="update =true" @click="getPelicula(item.id)">Actualizar Pelicula {{item.id}}</button>
                </div>
            </div>
        </div>
    </div>
        <!--<div v-for="c in peliculas" k:ey="c.id">
            <div >Nombre: {{c.nombre}} </div>
            <div >Descripcion: {{c.descripcion}} </div>
            <div><button @click="removePelicula(c.id)">eliminar {{c.id}}</button></div>
            
            <button v-on:click="update =true" @click="cargarPelicula(c.id)">Actualizar Pel�cula</button>
             
        </div>-->
        <button v-on:click="update =false">Crear Pelicula</button>
        <div v-if="update==true">
            <div class="col">
                <div class="card" style="width: 18rem;">
                    <div class="card-body">
                        <h1>ACTUALIZAR PELICULA</h1>
                        <form @submit="sendForm2">
                            <label>Nombre: </label>
                            <input v-model="pelicula.nombre" type="text" >
                            <label>Descripcion: </label>
                            <input v-model="pelicula.descripcion" type="text" />
                            <label>Imagen: ak</label>
                                   
                            <input v-model="pelicula.imagen" type="text" />
                            
          height="70" width="90"/>
			    <label>Precio: </label>
                            <input v-model="pelicula.precio" type="text" />
			    <label>Categorias: </label>
                            <input v-model="pelicula.categorias" type="text" />
			    <label>Horarios: </label>
                            <input v-model="pelicula.horarios" type="text" />
                            <!--<select  v-model="id">
                                <option v-for="c  in peliculas" ::key="c.id" :value="c.id">
                                    {{c.nombre}}
                                </option>
                            </select>-->
                            <input type="submit" value="Enviar" @click="getPeliculasRefresh()">
                            <button v-on:click="update =null">cancelar</button>
                        </form>
                        
                    </div>
                </div>
            </div>
        </div>
 
        <div v-if="update==false">
            <div class="col">
                <div class="card" style="width: 18rem;">
                    <div class="card-body">
                        <h1>REGISTRAR PELiCULA</h1>
                        <form @submit="sendForm">
                            <label>Nombre:</label>
                            <input v-model="pelicula.nombre" type="text" />
                            <label>Descripcion:</label>
                            <input v-model="pelicula.descripcion" type="text" />
			    <label>Imagen: </label>
                            <input v-model="pelicula.imagen" type="text" />
			    <label>Precio: </label>
                            <input v-model="pelicula.precio" type="text" />
			    <label>Categorias: </label>
                            <input v-model="pelicula.categorias" type="text" />
			    <label>Horarios: </label>
                            <input v-model="pelicula.horarios" type="text" />
                            <!--<select  v-model="id">
                                <option v-for="c  in peliculas" ::key="c.id" :value="c.id">
                                    {{c.nombre}}
                                </option>
                            </select>-->
                            <input type="submit" value="Enviar" @click="">
                            <button v-on:click="update =null">cancelar</button> 
                        </form>
                    </div>
            </div>
        </div>
    </div>
</template>
 
 <script>
 import axios from "axios";
 export default {
    props: {
        update: Boolean,
        formTitle: String,
        peliculaActualizar: [],
        pelicula: {
                nombre:"",
                descripcion:"",
		imagen:"",
		precio:"",
		categorias:"",
		horarios:"",
                textoABuscar: '',
            },         
    },
     data(){
         return {
            update: Boolean,
            peliculas: [],
            pelicula: {
                nombre:"",
                descripcion:"",
		imagen:"",
		precio:"",
		categorias:"",
		horarios:"",
                textoABuscar: '',
            }, 
            peliculaActualizar: [],    
            buscar: '',       
         };
     },
     methods: {
        sendForm(e){
            e.preventDefault();
            
            console.log(this.pelicula);
            const formData = new FormData();
            
            formData.append("nombre", this.pelicula.nombre);
            formData.append("descripcion", this.pelicula.descripcion);
            formData.append("imagen", this.pelicula.imagen);
            formData.append("precio", this.pelicula.precio);
            formData.append("categorias", this.pelicula.categorias);
            formData.append("horarios", this.pelicula.horarios);	 
            //this.insert(formData);
            this.addPelicula();
        },
        sendForm2(e){
            e.preventDefault();
            
            console.log(this.pelicula);
            const formData = new FormData();

            formData.append("id", this.pelicula.id);
            formData.append("nombre", this.pelicula.nombre);
            formData.append("descripcion", this.pelicula.descripcion);
            formData.append("imagen", this.pelicula.imagen);
            formData.append("precio", this.pelicula.precio);
            formData.append("categorias", this.pelicula.categorias);
            formData.append("horarios", this.pelicula.horarios);
            //this.insert(formData);
            this.guardar(this.pelicula.id);
        },
        insert(formData)
        {
            fetch("http://localhost:3000/pelicula/",{
                method: "POST",
                body: formData,
            })
                .then((res) =>res.json())
                .then((res)=>console.log(res));
        },
        async addPelicula() {
            const res = await axios.post(`http://localhost:3000/pelicula`, {
                nombre: this.pelicula.nombre,
                descripcion: this.pelicula.descripcion,
            });
            this.peliculas = [...this.peliculas, res.data];
            this.nombre = "";
            this.descripcion = "";
        },

       async guardar(id){

        axios.patch(`http://localhost:3000/pelicula/${id}`,
            { "nombre": this.pelicula.nombre, "descripcion": this.pelicula.descripcion }
           // { headers: { 'Content-Type': 'application/json', 'X-CSRF-TOKEN': crsfToken }, }
        ).then((response) => {
            // Code
            console.log("actualizado");
            this.peliculas = [...this.peliculas, res.data];
            this.nombre = "";
            this.descripcion = "";
            getPeliculasRefresh();
        }).catch((error) => {
            // Code
            //console.log(error);
        })
       
        },
        removePelicula(id) {
            axios.delete(`http://localhost:3000/pelicula/${id}`)
            this.peliculas = this.peliculas.filter(pelicula => pelicula.id !== id)
            console.log(pelicula);
        },
        async cargarPelicula(id){
            axios.get(`http://localhost:3000/pelicula/${id}`)
            this.peliculas = this.peliculas.filter(pelicula => pelicula.id !== id)
            return pelicula;
            console.log(pelicula.nombre + pelicula.descripcion);
        },
        getPeliculas() { //id
            axios({
                method: "get",
                //url:  `http://localhost:3000/pelicula/${id}`
                url:  "http://localhost:3000/pelicula/?q="+this.textoABuscar
                //process.env.VUE_APP_RUTA_API+"/tareas/?q="+this.textoABuscar
            })
                .then(response => {
                    this.peliculas = response.data;
                    console.log(response);
                })
                .catch(e => console.log(e));
        },
        getPeliculasRefresh() {
            axios({
                method: "get",
                url:  `http://localhost:3000/pelicula`
                //url: process.env.VUE_APP_RUTA_API+"/tareas/?q="+this.textoABuscar
            })
                .then(response => {
                    this.peliculas = response.data;
                    console.log(response);
                })
                .catch(e => console.log(e));
        },
        getPelicula(id){
            axios({
                method: "get",
                //url: process.env.VUE_APP_RUTA_API+"/tareas/"+this.$route.params.id
                url:  `http://localhost:3000/pelicula/${id}`
            })
            .then(response => {
                this.pelicula = response.data;
            console.log(response);
            })
            .catch(e => console.log(e));
        },
        async getMenorPrecio(precio){
            axios.get(`http://localhost:3000/pelicula`)
            .then(response => {
                    this.peliculas = response.data;
                    console.log(response);
                    this.peliculas = this.peliculas.filter(pelicula => pelicula.precio < precio)
                    return peliculas
                })
                .catch(e => console.log(e));
           ;
            //console.log(pelicula.nombre + pelicula.descripcion);
        },
     },
     computed: {

        items() {
      return this.peliculas.filter(item => {
        return item.nombre.toLowerCase().includes(this.buscar.toLowerCase());
      });
    },
     },
     mounted() {
        fetch("http://localhost:3000/pelicula")
        .then((res) =>res.json())
        .then((res)=>this.peliculas=res);
    },
     components: {
     }
     
 }
 </script>
 <style >
  .producto-relacionado-precio{
        background: orangered;
        color: white;
        text-align: center;
        padding: 10px; 
    }
 </style>