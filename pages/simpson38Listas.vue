<template>
  <v-container>
    <script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
    <h1>Metodo Simpson 3/8 Con listas</h1>
    <p class="text-lg-left">
      Dar cick en el botón con el simbolo mas(+) para agregar los puntos a la
      lista X y la lista Y
    </p>
    <v-row v-for="(textfield, i) in textfields" :key="i">
      <v-col cols="12" md="3">
        <v-text-field
          v-model="textfield.itemx"
          label="Item x"
          required
          type="number"
        ></v-text-field>
      </v-col>

      <v-col cols="12" md="3">
        <v-text-field
          v-model="textfield.itemy"
          label="Item y"
          required
          type="number"
        ></v-text-field>
      </v-col>

      <v-col cols="12" md="2">
        <v-btn @click="remove(i)" style="margin: 10px 0 5px 0" color="red"
          ><v-icon>mdi-delete</v-icon>
        </v-btn>
      </v-col>
    </v-row>
    <v-btn @click="add" color="teal"><v-icon dark>mdi-plus</v-icon></v-btn>
    <div style="min-height: 50px"></div>
    <v-btn class="mx-8" @click="simpson38Listas()"> Calcular integral </v-btn>
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
    <br />
  </v-container>
</template>

<script>
const axios = require('axios')
export default {
  data: () => ({
    textfields: [],
    resultado: '',
  }),

  methods: {
    add() {
      this.textfields.push({
        itemx: '',
        itemy: '',
      })
    },

    simpson38Listas() {
      const url = 'https://backend-metodos.herokuapp.com/simpson38Listas'
      var arreglox = []
      var arregloy = []
      this.textfields.forEach((element) => {
        arreglox.push(parseFloat(element.itemx))
        arregloy.push(parseFloat(element.itemy))
      })
      let data = {}
      data.x = arreglox
      data.fx = arregloy

      this.$axios
        .post(url, data)
        .then((res) => {
          this.resultado = res.data.res.toString()
          alert('Integral calculada')
          console.log('re', this.resultado)
          var lineDiv = document.getElementById('line-chart')
          var traceA = [
            {
              x: arreglox,
              y: arregloy,
              type: 'scatter',
              mode: 'lines',
            },
          ]
          var layout = {
            title: 'Grafica',
          }
          Plotly.newPlot(lineDiv, traceA, layout)
        })
        .catch((err) => {
          console.error(err)
        })
    },

    remove(index) {
      this.textfields.splice(index, 1)
    },
  },
}
</script>
