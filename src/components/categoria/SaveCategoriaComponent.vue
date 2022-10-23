<template>
    <update v-bind:update="false" form-title="Registrar"></update>
    <h1>Listado de categorias en componente</h1>
        <div v-for="c in categorias" k:ey="c.id">
            <div >Nombre: {{c.nombre}} </div>
            <div >Descripcion: {{c.descripcion}} </div>
            <div><button @click="removeCategoria(c.id)">eliminar {{c.id}}</button></div>
            <div><button v-bind:update.value="true"  form-title="Modificar" @click="updateMood()">Modificar {{c.id}}</button></div>
            <button v-on:click="update =true">update</button>
             
        </div>

        <div v-if="update==true">
            <div class="col">
        <div class="card" style="width: 18rem;">
            <div class="card-body">
                <h1>ACTUALIZAR CATEGORIA</h1>
                <form @submit="sendForm">
                    <label>Nombre:</label>
                    <input v-model="categoria.nombre" type="text" >
                    <label>Descripcion:</label>
                    <input v-model="categoria.descripcion" type="text" />
                    <!--<select  v-model="id">
                        <option v-for="c  in categorias" ::key="c.id" :value="c.id">
                            {{c.nombre}}
                        </option>
                    </select>-->
                    <input type="submit" value="Enviar" @click="">
                </form>
                
            </div>
        </div>
    </div>
        </div>
 

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
                </form>
                
            </div>
        </div>
    </div>
</template>
 
 <script>
 import axios from "axios";
 export default {
    props: {
        update: Boolean,
        formTitle: String
    },
     data(){
         return {
            update: Boolean,
            categorias: [],
            categoria: {
                nombre:"",
                descripcion:"",
            },            
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
        updateMood() {
            this.update="true";
            console.log(update);
        }
     },
     computed: {
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