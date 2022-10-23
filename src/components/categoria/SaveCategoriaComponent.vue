<template>
    <update v-bind:update="false" form-title="Registrar"></update>


    <h1>Listado de categorias en componente</h1>

    <input type="text" v-model="buscar" class="form-control" placeholder="Ejemplo: terror"/>

    <div class="row mt-3">
    
    <div class="col-md-4" v-for="item in categorias" v-bind:key="item.id"> 

        <div class="card mb-3">
            <!--<img class="card-img-top" v-bind:src="'img/' + item.img" v-bind:alt="item.nombre">-->
            <div class="card-body">
            <h3 class="card-title mb-3"><strong>Nombre:</strong>{{ item.nombre }}</h3>
            <p class="card-text"><strong>Descripcion:</strong> {{ item.descripcion }}</p>
   
            </div>
        </div>
    </div>
</div>
        <div v-for="c in categorias" k:ey="c.id">
            <div >Nombre: {{c.nombre}} </div>
            <div >Descripcion: {{c.descripcion}} </div>
            <div><button @click="removeCategoria(c.id)">eliminar {{c.id}}</button></div>
            
            <button v-on:click="update =true" @click="cargarCategoria(c.id)">Actualizar Categoria</button>
             
        </div>
        <button v-on:click="update =false">Crear Categoria</button>
        <div v-if="update==true">
            <div class="col">
                <div class="card" style="width: 18rem;">
                    <div class="card-body">
                        <h1>ACTUALIZAR CATEGORIA</h1>
                        <form @submit="sendForm2">
                            <label>Nombre: {{categoria.nombre}}</label>
                            <input v-model="categoria.nombre" type="text" >
                            <label>Descripcion: {{categoria.descripcion}}</label>
                            <input v-model="categoria.descripcion" type="text" />
                            <!--<select  v-model="id">
                                <option v-for="c  in categorias" ::key="c.id" :value="c.id">
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
 
        <div v-if="update==false">
            <div class="col">
                <div class="card" style="width: 18rem;">
                    <div class="card-body">
                        <h1>REGISTRAR CATEGORIA</h1>
                        <form @submit="sendForm">
                            <label>Nombre:</label>
                            <input v-model="categoria.nombre" type="text" />
                            <label>Descripcion:</label>
                            <input v-model="categoria.descripcion" type="text" />
                            <!--<select  v-model="id">
                                <option v-for="c  in categorias" ::key="c.id" :value="c.id">
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
        categoriaActualizar: []
    },
     data(){
         return {
            update: Boolean,
            categorias: [],
            categoria: {
                nombre:"",
                descripcion:"",
            }, 
            categoriaActualizar: [],    
            buscar: '',       
         };
     },
     methods: {
        sendForm(e){
            e.preventDefault();
            
            console.log(this.categoria);
            const formData = new FormData();
            
            formData.append("nombre", this.categoria.nombre);
            formData.append("descripcion", this.categoria.descripcion);

            //this.insert(formData);
            this.addCategoria();
        },
        insert(formData)
        {
            fetch("http://localhost:3000/categoria/",{
                method: "POST",
                body: formData,
            })
                .then((res) =>res.json())
                .then((res)=>console.log(res));
        },
        async addCategoria() {
            const res = await axios.post(`http://localhost:3000/categoria`, {
                nombre: this.categoria.nombre,
                descripcion: this.categoria.descripcion,
            });
            this.categorias = [...this.categorias, res.data];
            this.nombre = "";
            this.descripcion = "";
        },
        async updateCategoria(id) {
            try {
                await axios.patch(`${`http://localhost:3000/categoria`}/${id}`, {
                    updateCategoria: true
                });
                this.categorias = this.categorias.map(categoria => {
                    if (categoria.id === id) {
                        categoria.updateCategoria = true;
                    }
                    return categoria;
                });
            } catch (error) {
                console.error(error);
            }
        },
        removeCategoria(id) {
            axios.delete(`http://localhost:3000/categoria/${id}`)
            this.categorias = this.categorias.filter(categoria => categoria.id !== id)
        },
        async cargarCategoria(id){
            try {
                await axios.get(`${`http://localhost:3000/categoria`}/${id}`, {
                    updateCategoria: true
                });
                this.categorias = this.categorias.map(categoria => {
                    if (categoria.id === id) {
                        categoria.updateCategoria = true;
                        //console.log(categoria);
                        categoriaActualizar=categoria
                    }
                    console.log(categoriaActualizar);
                    return categoriaActualizar;
                });
            } catch (error) {
                console.error(error);
            }
            
        }
     },
     computed: {

        items() {
      return categorias.filter(categorias => {
        return categoria.nombre.toLowerCase().includes(this.buscar.toLowerCase());
        console.log( categoria.nombre.toLowerCase().includes(this.buscar.toLowerCase()));
      });
    },
     },
     mounted() {
        fetch("http://localhost:3000/categoria")
        .then((res) =>res.json())
        .then((res)=>this.categorias=res);
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