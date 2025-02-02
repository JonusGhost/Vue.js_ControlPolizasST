<template>
    <v-card>
      <v-layout>
        <v-app-bar color="primary">
          <v-app-bar-nav-icon variant="text" @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
  
          <v-toolbar-title>My files</v-toolbar-title>
  
          <v-spacer></v-spacer>
  
          <template v-if="$vuetify.display.mdAndUp">
            <v-btn icon="mdi-magnify" variant="text"></v-btn>
  
            <v-btn icon="mdi-filter" variant="text"></v-btn>
          </template>
  
          <v-btn icon="mdi-dots-vertical" variant="text"></v-btn>
        </v-app-bar>
  
        <v-navigation-drawer
          v-model="drawer"
          :location="$vuetify.display.mobile ? 'bottom' : undefined"
          temporary
        >
          <v-list>
            <v-list-item
            v-for="(item, index) in items"
            :key="index"
            link
            @click="navigateTo(item.route)"
            >
                <v-list-item-title>{{ item.title }}</v-list-item-title>
            </v-list-item>
        </v-list>

        </v-navigation-drawer>
  
        <v-main style="height: 500px;">
          <v-card-text>
            The navigation drawer will appear from the bottom on smaller size screens.
          </v-card-text>
        </v-main>
      </v-layout>
    </v-card>
  </template>
  <script>
    export default {
      data: () => ({
        drawer: false,
        group: null,
        items: [
          {
            title: 'Clientes',
            value: '/clientes',
          },
          {
            title: 'Polizas',
            value: '/polizas',
          },
        ],
      }),
  
      watch: {
        group () {
          this.drawer = false
        },
      },

      methods: {
        navigateTo(route) {
            this.$router.push(route);
        },
        save() {
            console.log('Datos guardados: ', this.formData);
        },
    },
    }
  </script>