<template>
  <!-- Layout de barra de navegación para las páginas -->
  <!-- Layout Default para el proveedor -->
  <div>
    <v-app>
      <v-navigation-drawer v-model="drawer" app>
        <v-list-item>
          <v-list-item-content>
            <v-list-item-title>Menú</v-list-item-title>
          </v-list-item-content>
        </v-list-item>

        <v-divider></v-divider>

        <v-list dense>
          <v-list-item
            :to="item.route"
            v-for="item in items_menu"
            :key="item.id"
          >
            <v-list-item-icon>
              <v-icon>{{ item.icon }}</v-icon>
            </v-list-item-icon>

            <v-list-item-content>
              <v-list-item-title style="padding: 5px">{{
                item.title
              }}</v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-navigation-drawer>

      <v-app-bar app color="#0096b8" dense
        ><v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon
        ><v-toolbar-title>Entrega Final Métodos Númericos</v-toolbar-title
        ><v-spacer></v-spacer><v-btn @click="logout()">Salir</v-btn></v-app-bar
      >
      <v-main
        ><v-container> <nuxt /> </v-container
      ></v-main>
    </v-app>
  </div>
</template>

<script>
export default {
  beforeMount() {},
  data() {
    return {
      drawer: null,
      items_menu: [
        {
          id: '01',
          title: 'Metodo 1',
          icon: 'mdi-chart-box',
          route: '/metodo1_2',
        },
        {
          id: '02',
          title: 'Metodo 2',
          icon: 'mdi-clipboard-edit',
          route: '/metodo2',
        },
      ],
    }
  },
  methods: {},
}
</script>
