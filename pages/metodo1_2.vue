<template>
  <!--Se visualizan los proveedores disponibles conjunto con sus respectivos servicios y rango de fechas de disponibilidad-->
  <div>
    <v-data-table
      :headers="headers"
      :items="metodos"
      :items-per-page="10"
      class="elevation-1"
      max-width="600"
    >
      <template v-slot:[`item.actions`]="{ item }">
        <v-icon class="mdi-gesture-tap-box" @click="validarMetodo(item)">
          mdi-gesture-tap-box
        </v-icon>
      </template>
    </v-data-table>
  </div>
</template>

<script>
const axios = require('axios')
export default {
  beforeMount() {
    this.loadMethods()
  },
  data() {
    return {
      cellphone: '',
      users: [],
      metodos: [],
      headers: [
        { text: 'Nombre Metodo', value: 'nombre_metodo' },
        { text: 'Descripcion', value: 'descripcion' },
        { text: 'Opciones', value: 'actions' },
      ],
      dialogContact: false,
      metodos2: [
        {
          nombre_metodo: 'Simpson 3/8',
          descripcion: 'Calcuar una integral con una función',
          numero: 1,
        },
        {
          nombre_metodo: 'Simpson 1/3 Multiple',
          descripcion: 'Calcuar una integral con una función',
          numero: 2,
        },
        {
          nombre_metodo: 'Simpson 3/8 Listas',
          descripcion: 'Calcuar una integral con dos listas',
          numero: 3,
        },
        {
          nombre_metodo: 'Simpson 1/3 Multiple Listas',
          descripcion:'Calcuar una integral con dos listas',
          numero: 4,
        },
      ],
    }
  },

  methods: {
    loadMethods() {
      this.metodos = this.metodos2
    },
    validarMetodo(metodo) {
      switch (metodo.numero) {
        case 1:
          console.log('Simpson 3/8')
          this.$router.push('/Simpson38')
          break
        case 2:
          console.log('Simpson 1/3 Multiple')
         this.$router.push('/Simpson13Multiple')
          break
        case 3:
          console.log('Simpson 3/8 Listas')
         this.$router.push('/Simpson38Listas')
          break
        case 4:
          console.log('Simpson 1/3 Multiple Listas')
          this.$router.push('/simpson13MultipleLista')
          break
      }
    },
    updateScreen() {
      window.location.reload()
    },
  },
}
</script>
