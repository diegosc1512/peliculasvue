<template>
    <div class="home">
      <!--<ListHorarios/>-->
      <SaveHorarioComponent/>
    </div>
  </template>
  
  <script>
    //import ListHorarios from '@/components/horario/ListhorariosComponente.vue'
    import Savehorario from '@/components/horario/SavehorarioComponent.vue'
import SavehorarioComponent from '@/components/horario/SavehorarioComponent.vue';
  
  export default {
    name: 'horarioView',
    components: {
    //Listhorarios,
    Savehorario,
    SavehorarioComponent
}
  }
  </script>