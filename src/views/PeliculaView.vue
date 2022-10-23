<template>
  <div class="home">
      <ListCategoria :categoria=categorias  ></ListCategoria>
      <div class="container ">
        <!--<div v-for="c in categorias.data" k:ey="c.id">
            {c.nombre,
            c.descripcion
            }
            <div >Nombre: {{categorias.nombre}} </div>
            <div >Descripcion: {{categorias.descripcion}} </div>
        </div>-->
        <!--<div class="row">
            <h4>Productos relacionados</h4>
        </div>
        <div class="row">
          <ProductoTarjeta class="clic" v-for="productoR of productosRelacionados"  @click="cambiar(productoR.id)" :producto="productoR"></ProductoTarjeta>
        </div>-->
      </div>
    </div>
  </template>
  
  <script>
//import ProductoPrincipal from '@/components/ProductoPrincipal.vue';
//import ProductoTarjeta from '@/components/ProductoTarjeta.vue';

import ListCategoria from '@/components/categoria/ListCategoriasComponente.vue'

  export default {
    name: 'PeliculaView',
    data(){
      return {
          categorias: [], 
          peliculas:[],
          productosRelacionados:[],
          producto:{},
          precioEstilos: "background: orangered; color: white; font-weight: bold"
      }
    },
    mounted() {
        fetch("http://localhost:3000/categoria")
        .then((res) =>res.json())
        .then((res)=>this.categorias=res);
        this.getPeliculas();
    },
    methods: {
      getPeliculas(){
          if(typeof this.$route.params.id == 'undefined')
          {
              axios({
                method: "get",
                url: "http://localhost:3000/categoria"
              })
              .then(response => {
                  this.categorias = response.data;
                  this.peliculas = response.data;
                  this.producto=this.peliculas[0];
                 // this.productosRelacionados=this.peliculas.filter((x) => x.id!= this.producto.id);
                  console.log(response);
              })
              .catch(e => console.log(e));
 
          }
          else
          {
            axios({
                method: "get",
                url: "http://localhost:3000/categoria"
              })
              .then(response => {
                  this.categorias = response.data;
                  this.producto=this.propeliculasductos.filter((x) => x.id== this.$route.params.id)[0];
                  this.productosRelacionados=this.peliculas.filter((x) => x.id!= this.$route.params.id);
                  console.log(response);
              })
              .catch(e => console.log(e));
          }
          
      },
      cambiar(idNuevo){
        this.$store.state.cantidad = 1;
        this.$store.state.color = "";
        this.$store.state.botonEstado = true;

        this.producto=this.peliculas.filter((x) => x.id== idNuevo)[0];
        this.productosRelacionados=this.peliculas.filter((x) => x.id!= idNuevo);
        this.$router.push({name: 'productoId', params: {id: idNuevo}});
      }
    },
    mounted(){
      this.getPeliculas()
    },
    components: {
      //ProductoPrincipal,ProductoTarjeta
      ListCategoria
    }
  }
  </script>
  <style>
  .container{
    margin-top: 50px;
  }
  
  .clic{
        cursor: pointer;
    }
  </style>