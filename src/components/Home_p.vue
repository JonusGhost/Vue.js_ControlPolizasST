<template>
    <v-card>
      <v-layout>
        <v-app-bar color="primary" prominent>
          <v-app-bar-nav-icon variant="text" @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
          <v-toolbar-title>Polizas</v-toolbar-title>  
          <v-spacer></v-spacer>
        </v-app-bar>
  
        <v-navigation-drawer v-model="drawer" :location="$vuetify.display.mobile ? 'bottom' : undefined" temporary>
          <v-list>
            <v-list-item v-for="(item, index) in items" :key="index" link @click="navigateTo(item.route)">
                <v-list-item-title>{{ item.title }}</v-list-item-title>
              </v-list-item>
          </v-list>
        </v-navigation-drawer>
  
         <v-main style="height: 95vh;">
          <v-card-text>
              <v-container>
                  <v-row>
                      <v-col cols="12">
                          <v-card>
                              <v-card-title class="text-h6">Registrar Poliza</v-card-title>
                              <v-divider></v-divider>
                              <v-card-text>
                                  <v-form ref="form" v-model="valid" lazy-validation>
                                      <v-text-field v-model="formData.total_horas" label="Hora" type="number" required></v-text-field>
                                      <v-text-field v-model="formData.fecha_inicio" label="Fecha inicio" type="date" required></v-text-field>
                                      <v-text-field v-model="formData.fecha_fin" label="Fecha fin" type="date" required></v-text-field>
                                      <v-text-field v-model="formData.precio" label="Precio" type="number"></v-text-field>
                                      <v-text-field v-model="formData.id_cliente" label="Cliente" required></v-text-field>
                                      <v-textarea v-model="formData.observaciones" label="Observaciones" type="text" required></v-textarea>
                                      <v-btn :disabled="!valid" color="primary" class="mt=4" @click="save">Guardar</v-btn>
                                  </v-form>
                              </v-card-text>
                          </v-card>
                      </v-col>
                  </v-row>
              </v-container>
          </v-card-text>
         </v-main>
      </v-layout>
    </v-card>
  </template>
  
  <script>
//  import { options } from 'node_modules/axios/index.cjs';
import axios from '../config/axios';

  export default {
    data: () => ({
      drawer: false,
      group: null,
      items: [
        {
          title: 'Clientes',
          value: `/home`,
        },
        {
          title: 'Polizas',
          value: '/polizas',
        },
      ],
      valid: false,
      formData: {
        total_horas: '',
        fecha_inicio: '',
        fecha_fin: '',
        precio: '',
        id_cliente: '',
        observaciones: ''
      },
    }),

    watch: {
      group() {
        this.drawer = false;
      },
    },

    created() {
      const id = this.$route.params.id;

      if (id) {
        this.loadPoliza(id);
      }
    },

    methods: {
      navigateTo(route) {
        this.$router.push(route);
      },
      save() {
        axios.post('poliza/guardar', this.formData)
          .then(response => {
            this.$refs.form.reset();
            this.formData.id = 0;
            this.$router.push('/polizas');
          }).catch(error => {
            console.error('Ocurrió un error:', error);
          });
        },
        loadPoliza(id){
          axios.get(`poliza/${id}`)
          .then(responce => {
            this.formData = { ...responce.data};
          })
          .catch(error => console.error('Error al obtener polzia:', error))
        },
        fetchOptions(){
            axios.get('nombre')
            .then(responce => {
                this.options = responce.data;
            })
            .catch(error => console.error('Error al cargar opciones:', error))
        }
    }
  }
</script>