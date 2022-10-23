<template>
<app-menu :menus=menus :menuColor=menuColor :marca=marca></app-menu>
<router-view/>

<list-categoria></list-categoria>
<app-footer :footerColor=footerColor></app-footer>
</template>

<script>
import AppMenu from '@/components/AppMenu.vue';
import ListCategoria from '@/components/categoria/ListCategoriasComponente.vue';
import AppFooter from '@/components/AppFooter.vue';
export default {
  name: 'app',
  data() {
    return {
      footerColor: "slategrey",
      menuColor: "lightblue",
      marca: "MegaDron",
      menus:[
        {
          etiqueta: "Inicio",
          url: "?"
        },
        {
          etiqueta: "Tienda",
          url: "?"
      }
      ]

  }
   
  },
  methods: {
  },
  computed: {
  },
  mounted() {

  },
  components: {
    AppMenu,
   // ListCategoria,
    AppFooter
  }
}
</script>



<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
