<template>
  <!-- muestra los campos necesarios para la creación y activación de una cuenta de acceso de un usuario-->
  <div>
    <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
    <v-form ref="formRegister" v-model="formRegister" lazy-validation>
      <!-- Se crea el formulario de regsitro de usuario -->

      <v-container>
        <v-row>
          <v-col cols="12" sm="6">
            <v-text-field
              v-model="funcion.funcion"
              :counter="100"
              :rules="funcionRules"
              label="Funcion principal"
              class="mt-md-6 px-md-3"
              required
            ></v-text-field>
          </v-col>

          <v-col cols="12" sm="6">
            <v-text-field
              v-model="funcion.funcion_derivada"
              :counter="100"
              :rules="funcionRules"
              label="Funcion para aproximar"
              class="mt-md-6 px-md-3"
              required
            ></v-text-field>
          </v-col>
        </v-row>
      </v-container>

      <v-container>
        <v-row>
          <v-col cols="18" sm="2.5">
            <v-text-field
              v-model="funcion.x_inicial"
              :counter="20"
              :rules="funcionRules"
              label="X inicial"
              class="mt-md-6 px-md-7"
              type="number"
              required
            ></v-text-field>
          </v-col>

          <v-col cols="18" sm="2.5">
            <v-text-field
              v-model="funcion.y_inicial"
              :counter="20"
              :rules="funcionRules"
              label="Y inicial"
              class="mt-md-6 px-md-6"
              type="number"
              required
            ></v-text-field>
          </v-col>

          <v-col cols="18" sm="2.5">
            <v-text-field
              v-model="funcion.delta"
              :counter="15"
              :rules="funcionRules"
              label="Delta (h)"
              class="mt-md-6 px-md-6"
              type="number"
              required
            ></v-text-field>
          </v-col>

          <v-col cols="18" sm="2.5">
            <v-text-field
              v-model="funcion.punto_x_inicial"
              :counter="20"
              :rules="funcionRules"
              label="Punto x inicial"
              class="mt-md-6 px-md-6"
              type="number"
              required
            ></v-text-field>
          </v-col>

          <v-col cols="18" sm="2.5">
            <v-text-field
              v-model="funcion.punto_x_final"
              :counter="20"
              :rules="funcionRules"
              label="Punto x final"
              class="mt-md-6 px-md-6"
              type="number"
              required
            ></v-text-field>
          </v-col>
        </v-row>
      </v-container>
      <div style="min-height: 100px"></div>
      <v-btn class="mx-4" @click="Euler"> Euler </v-btn>
      <v-btn class="mr-4" @click="Heun"> Heun </v-btn>
      <v-btn class="mr-4" @click="Punto_medio"> Punto medio </v-btn>
      <v-btn class="mr-4" @click="Ralston"> Ralston </v-btn>
      <v-btn class="mr-4" @click="Rk3"> Runge kutta 3er orden </v-btn>
      <v-btn class="mr-4" @click="Rk4"> Runge kutta 4to orden </v-btn>
      <v-btn class="mr-4" @click="Butcher"> Butcher </v-btn>
      <v-btn class="mx-4 mt-8" @click="Todas"> Todas </v-btn
      ><!--  -->

      <container>
        <div id="line-chart" class="mt-9"></div>
      </container>

      <container>
        <div id="espacio" class="mt-9"></div>
      </container>
    </v-form>

    <!-- Le informa al usuario de la omisión o error de un campo necesario para la realización del registro-->
    <v-dialog v-model="dialog" max-width="290">
      <v-card>
        <v-card-title class="headline"> Error </v-card-title>

        <v-card-text> Llene todos los campos </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn color="accent" text @click="dialog = false"> Cerrar </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>
<script>
const axios = require('axios')
export default {
  layout: 'blank',
  beforeMount() {},
  data: () => ({
    /*Reglas para los campos requeridos en el formulario de registro*/
    valid: true,
    formRegister: null,
    funcionRules: [
      (v) => !!v || 'Complete correctamente el campo',
      (v) => v && v.length >= 1 && v.length != 0,
    ],

    funciones: [],
    funcion: {
      funcion: null,
      funcion_derivada: null,
      x_inicial: null,
      y_inicial: null,
      delta: null,
      punto_x_inicial: null,
      punto_x_final: null,
    },
    dialog: false,
  }),

  methods: {
    Euler() {
      if (this.$refs.formRegister.validate() && this.formRegister) {
        const url = 'https://backend-metodos.herokuapp.com/Euler'
        let data = {}
        data.funcion = this.funcion.funcion.toString()
        data.funcion_derivada = this.funcion.funcion_derivada.toString()
        data.x_inicial = parseFloat(this.funcion.x_inicial)
        data.y_inicial = parseFloat(this.funcion.y_inicial)
        data.delta = parseFloat(this.funcion.delta)
        data.punto_x_inicial = parseFloat(this.funcion.punto_x_inicial)
        data.punto_x_final = parseFloat(this.funcion.punto_x_final)

        this.$axios
          .post(url, data)
          .then((res) => {
            console.log(res.data.Euler)
            var lineDiv = document.getElementById('line-chart')
            var valoresx = res.data.x
            var valoresy = res.data.y

            var funx = res.data.arreglox
            var funy = res.data.arregloy
            var data = [
              { x: funx, y: funy, mode: 'lines', name: 'Funcion principal' },
              {
                x: valoresx,
                y: valoresy,
                mode: 'lines',
                name: 'Funcion derivada',
              },
            ]
            var layout = {
              title: 'Grafica de ' + res.data.name,
            }
            Plotly.newPlot(lineDiv, data, layout)
          })
          .catch((err) => {
            console.error(err)
          })
        console.log('name', name)
      } else {
        this.dialog = true
      }
    },

    Heun() {
      if (this.$refs.formRegister.validate() && this.formRegister) {
        const url = 'https://backend-metodos.herokuapp.com/Heun'
        let data = {}
        data.funcion = this.funcion.funcion.toString()
        data.funcion_derivada = this.funcion.funcion_derivada.toString()
        data.x_inicial = parseFloat(this.funcion.x_inicial)
        data.y_inicial = parseFloat(this.funcion.y_inicial)
        data.delta = parseFloat(this.funcion.delta)
        data.punto_x_inicial = parseFloat(this.funcion.punto_x_inicial)
        data.punto_x_final = parseFloat(this.funcion.punto_x_final)

        this.$axios
          .post(url, data)
          .then((res) => {
            console.log(res.data.Euler)
            var lineDiv = document.getElementById('line-chart')
            var valoresx = res.data.x
            var valoresy = res.data.y

            var funx = res.data.arreglox
            var funy = res.data.arregloy
            var data = [
              { x: funx, y: funy, mode: 'lines', name: 'Funcion principal' },
              {
                x: valoresx,
                y: valoresy,
                mode: 'lines',
                name: 'Funcion derivada',
              },
            ]
            var layout = {
              title: 'Grafica de ' + res.data.name,
            }
            Plotly.newPlot(lineDiv, data, layout)
          })
          .catch((err) => {
            console.error(err)
          })
        console.log('name', name)
      } else {
        this.dialog = true
      }
    },

    Punto_medio() {
      if (this.$refs.formRegister.validate() && this.formRegister) {
        const url = 'https://backend-metodos.herokuapp.com/Punto_medio'
        let data = {}
        data.funcion = this.funcion.funcion.toString()
        data.funcion_derivada = this.funcion.funcion_derivada.toString()
        data.x_inicial = parseFloat(this.funcion.x_inicial)
        data.y_inicial = parseFloat(this.funcion.y_inicial)
        data.delta = parseFloat(this.funcion.delta)
        data.punto_x_inicial = parseFloat(this.funcion.punto_x_inicial)
        data.punto_x_final = parseFloat(this.funcion.punto_x_final)

        this.$axios
          .post(url, data)
          .then((res) => {
            var lineDiv = document.getElementById('line-chart')
            var valoresx = res.data.x
            var valoresy = res.data.y

            var funx = res.data.arreglox
            var funy = res.data.arregloy
            var data = [
              { x: funx, y: funy, mode: 'lines', name: 'Funcion principal' },
              {
                x: valoresx,
                y: valoresy,
                mode: 'lines',
                name: 'Funcion derivada',
              },
            ]
            var layout = {
              title: 'Grafica de ' + res.data.name,
            }
            Plotly.newPlot(lineDiv, data, layout)
          })
          .catch((err) => {
            console.error(err)
          })
        console.log('name', name)
      } else {
        this.dialog = true
      }
    },

    Ralston() {
      if (this.$refs.formRegister.validate() && this.formRegister) {
        const url = 'https://backend-metodos.herokuapp.com/Ralston'
        let data = {}
        data.funcion = this.funcion.funcion.toString()
        data.funcion_derivada = this.funcion.funcion_derivada.toString()
        data.x_inicial = parseFloat(this.funcion.x_inicial)
        data.y_inicial = parseFloat(this.funcion.y_inicial)
        data.delta = parseFloat(this.funcion.delta)
        data.punto_x_inicial = parseFloat(this.funcion.punto_x_inicial)
        data.punto_x_final = parseFloat(this.funcion.punto_x_final)

        this.$axios
          .post(url, data)
          .then((res) => {
            var lineDiv = document.getElementById('line-chart')
            var valoresx = res.data.x
            var valoresy = res.data.y

            var funx = res.data.arreglox
            var funy = res.data.arregloy
            var data = [
              { x: funx, y: funy, mode: 'lines', name: 'Funcion principal' },
              {
                x: valoresx,
                y: valoresy,
                mode: 'lines',
                name: 'Funcion derivada',
              },
            ]
            var layout = {
              title: 'Grafica de ' + res.data.name,
            }
            Plotly.newPlot(lineDiv, data, layout)
          })
          .catch((err) => {
            console.error(err)
          })
        console.log('name', name)
      } else {
        this.dialog = true
      }
    },

    Rk3() {
      if (this.$refs.formRegister.validate() && this.formRegister) {
        const url =
          'https://backend-metodos.herokuapp.com/Runge_kutta_3er_orden'
        let data = {}
        data.funcion = this.funcion.funcion.toString()
        data.funcion_derivada = this.funcion.funcion_derivada.toString()
        data.x_inicial = parseFloat(this.funcion.x_inicial)
        data.y_inicial = parseFloat(this.funcion.y_inicial)
        data.delta = parseFloat(this.funcion.delta)
        data.punto_x_inicial = parseFloat(this.funcion.punto_x_inicial)
        data.punto_x_final = parseFloat(this.funcion.punto_x_final)

        this.$axios
          .post(url, data)
          .then((res) => {
            var lineDiv = document.getElementById('line-chart')
            var valoresx = res.data.x
            var valoresy = res.data.y

            var funx = res.data.arreglox
            var funy = res.data.arregloy
            var data = [
              { x: funx, y: funy, mode: 'lines', name: 'Funcion principal' },
              {
                x: valoresx,
                y: valoresy,
                mode: 'lines',
                name: 'Funcion derivada',
              },
            ]
            var layout = {
              title: 'Grafica de ' + res.data.name,
            }
            Plotly.newPlot(lineDiv, data, layout)
          })
          .catch((err) => {
            console.error(err)
          })
        console.log('name', name)
      } else {
        this.dialog = true
      }
    },

    Rk4() {
      if (this.$refs.formRegister.validate() && this.formRegister) {
        const url =
          'https://backend-metodos.herokuapp.com/Runge_kutta_4to_orden'
        let data = {}
        data.funcion = this.funcion.funcion.toString()
        data.funcion_derivada = this.funcion.funcion_derivada.toString()
        data.x_inicial = parseFloat(this.funcion.x_inicial)
        data.y_inicial = parseFloat(this.funcion.y_inicial)
        data.delta = parseFloat(this.funcion.delta)
        data.punto_x_inicial = parseFloat(this.funcion.punto_x_inicial)
        data.punto_x_final = parseFloat(this.funcion.punto_x_final)

        this.$axios
          .post(url, data)
          .then((res) => {
            var lineDiv = document.getElementById('line-chart')
            var valoresx = res.data.x
            var valoresy = res.data.y

            var funx = res.data.arreglox
            var funy = res.data.arregloy
            var data = [
              { x: funx, y: funy, mode: 'lines', name: 'Funcion principal' },
              {
                x: valoresx,
                y: valoresy,
                mode: 'lines',
                name: 'Funcion derivada',
              },
            ]
            var layout = {
              title: 'Grafica de ' + res.data.name,
            }
            Plotly.newPlot(lineDiv, data, layout)
          })
          .catch((err) => {
            console.error(err)
          })
        console.log('name', name)
      } else {
        this.dialog = true
      }
    },

    Butcher() {
      if (this.$refs.formRegister.validate() && this.formRegister) {
        const url =
          'https://backend-metodos.herokuapp.com/Runge_kutta_orden_superior'
        let data = {}
        data.funcion = this.funcion.funcion.toString()
        data.funcion_derivada = this.funcion.funcion_derivada.toString()
        data.x_inicial = parseFloat(this.funcion.x_inicial)
        data.y_inicial = parseFloat(this.funcion.y_inicial)
        data.delta = parseFloat(this.funcion.delta)
        data.punto_x_inicial = parseFloat(this.funcion.punto_x_inicial)
        data.punto_x_final = parseFloat(this.funcion.punto_x_final)

        this.$axios
          .post(url, data)
          .then((res) => {
            var lineDiv = document.getElementById('line-chart')
            var valoresx = res.data.x
            var valoresy = res.data.y

            var funx = res.data.arreglox
            var funy = res.data.arregloy
            var data = [
              { x: funx, y: funy, mode: 'lines', name: 'Funcion principal' },
              {
                x: valoresx,
                y: valoresy,
                mode: 'lines',
                name: 'Funcion derivada',
              },
            ]
            var layout = {
              title: 'Grafica de ' + res.data.name,
            }
            Plotly.newPlot(lineDiv, data, layout)
          })
          .catch((err) => {
            console.error(err)
          })
        console.log('name', name)
      } else {
        this.dialog = true
      }
    },

    Todas() {
      if (this.$refs.formRegister.validate() && this.formRegister) {
        const url = 'https://backend-metodos.herokuapp.com/Todas'
        let data = {}
        data.funcion = this.funcion.funcion.toString()
        data.funcion_derivada = this.funcion.funcion_derivada.toString()
        data.x_inicial = parseFloat(this.funcion.x_inicial)
        data.y_inicial = parseFloat(this.funcion.y_inicial)
        data.delta = parseFloat(this.funcion.delta)
        data.punto_x_inicial = parseFloat(this.funcion.punto_x_inicial)
        data.punto_x_final = parseFloat(this.funcion.punto_x_final)

        this.$axios
          .post(url, data)
          .then((res) => {
            console.log(res.data.toString())
            var lineDiv = document.getElementById('line-chart')
            var valoresx = res.data.xres
            var valoresy = res.data.yres
            var names = res.data.names

            var funx = res.data.arreglox
            var funy = res.data.arregloy
            var data = [
              { x: funx, y: funy, mode: 'lines', name: 'Funcion principal' },
              {
                x: valoresx[0],
                y: valoresy[0],
                mode: 'lines',
                name: names[0],
              },
              { x: valoresx[1], y: valoresy[1], mode: 'lines', name: names[1] },
              { x: valoresx[2], y: valoresy[2], mode: 'lines', name: names[2] },
              { x: valoresx[3], y: valoresy[3], mode: 'lines', name: names[3] },
              { x: valoresx[4], y: valoresy[4], mode: 'lines', name: names[4] },
              { x: valoresx[5], y: valoresy[5], mode: 'lines', name: names[5] },
            ]
            var layout = {
              title: 'Todas las graficas',
            }
            Plotly.newPlot(lineDiv, data, layout)
          })
          .catch((err) => {
            console.error(err)
          })
        console.log('name', name)
      } else {
        this.dialog = true
      }
    },
  },
}
</script>
