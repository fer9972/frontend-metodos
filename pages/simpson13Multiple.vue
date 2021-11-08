<template>
  <!-- muestra los campos necesarios para la creación y activación de una cuenta de acceso de un usuario-->
  <div>
    <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
    <v-form ref="formRegister" v-model="formRegister" lazy-validation>
      <!-- Se crea el formulario de regsitro de usuario -->

      <v-container>
        <h1>Metodo Simpson 1/3 Multiple con una función</h1>
        <p class="text-lg-left">
          Por Favor ingrese la función que desea calcular, y los parámetros X0,
          Xn y n(Recuerde que n tiene que ser par)
        </p>
        <v-row>
          <v-col cols="12" sm="6">
            <v-text-field
              v-model="funcion.funcion"
              :counter="100"
              :rules="funcionRules"
              label="Funcion"
              class="mt-md-6 px-md-6"
              required
            ></v-text-field>
          </v-col>
        </v-row>
      </v-container>

      <v-container>
        <v-row>
          <v-col cols="18" sm="2.5">
            <v-text-field
              v-model="funcion.xn"
              :counter="20"
              :rules="funcionRules"
              label="Xn"
              class="mt-md-6 px-md-9"
              type="number"
              required
            ></v-text-field>
          </v-col>

          <v-col cols="18" sm="2.5">
            <v-text-field
              v-model="funcion.x0"
              :counter="20"
              :rules="funcionRules"
              label="X0"
              class="mt-md-6 px-md-6"
              type="number"
              required
            ></v-text-field>
          </v-col>

          <v-col cols="18" sm="2.5">
            <v-text-field
              v-model="funcion.n"
              :counter="20"
              :rules="funcionRules"
              label="n"
              class="mt-md-6 px-md-6"
              type="number"
              required
            ></v-text-field>
          </v-col>
        </v-row>
      </v-container>

      <div style="min-height: 50px"></div>
      <v-btn class="mx-8" @click="simpson13Multiple()">
        Calcular integral
      </v-btn>

      <v-container>
        <v-row>
          <v-col cols="12" sm="6">
            <v-text-field
              v-model="resultado"
              label="Resultado"
              class="mt-md-6 px-md-6"
              disabled
              required
            ></v-text-field>
          </v-col>
        </v-row>
      </v-container>

      <container>
        <div id="line-chart"></div>
      </container>
      <!--  -->
    </v-form>
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
      funcion: '',
      xn: null,
      x0: null,
      n: null,
    },
    resultado: '',
    dialog: false,
  }),

  methods: {
    simpson13Multiple() {
      //TODO
      if (this.$refs.formRegister.validate() && this.formRegister) {
        const url = 'https://backend-metodos.herokuapp.com/simpson13Multiple'
        let data = {}
        data.funcion = this.funcion.funcion.toString()
        data.xn = parseFloat(this.funcion.xn)
        data.x0 = parseFloat(this.funcion.x0)
        data.n = parseFloat(this.funcion.n)
        if (data.n % 2 != 0) {
          alert('recuerda que el n tiene que ser par')
        } else {
          this.$axios
            .post(url, data)
            .then((res) => {
              console.log(res)
              alert('Integral Calculada')
              this.resultado = res.data.res.toString()
              var lineDiv = document.getElementById('line-chart')
              var valoresx = res.data.arreglox
              var valoresy = res.data.arregloy
              var data = [{ x: valoresx, y: valoresy, mode: 'lines' }]
              var layout = { title: 'Grafica de la función' }
              Plotly.newPlot(lineDiv, data, layout)
            })
            .catch((err) => {
              console.error(err)
            })
          console.log('name', name)
        }
      } else {
        this.dialog = true
      }
    },
  },
}
</script>
