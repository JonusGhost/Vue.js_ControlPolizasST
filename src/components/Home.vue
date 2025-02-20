<template>
    <v-card>
      <v-layout>
        <v-app-bar color="primary" prominent>
          <v-app-bar-nav-icon variant="text" @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
          <v-toolbar-title>Clientes</v-toolbar-title>  
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
                              <v-card-title class="text-h6">Nuevo Cliente</v-card-title>
                              <v-divider></v-divider>
                              <v-card-text>
                                  <v-form ref="form" v-model="valid" lazy-validation>
                                      <v-text-field v-model="formData.name" label="Nombre" required></v-text-field>
                                      <v-text-field v-model="formData.email" label="Correo Electronico" type="email" required></v-text-field>
                                      <v-text-field v-model="formData.password" label="Contrasena" type="password" required></v-text-field>
                                      <v-text-field v-model="formData.rfc" label="RFC" maxlength="13"></v-text-field>
                                      <v-text-field v-model="formData.contacto" label="Contacto" required></v-text-field>
                                      <v-text-field v-model="formData.telefono_contacto" label="Telefono de contacto" required></v-text-field>
                                      <v-textarea v-model="formData.direccion" label="Direccion"></v-textarea>
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
  import axios from '../config/axios';

    export default {
      data: () => ({
        drawer: false,
        group: null,
        items: [
              {
                title: 'Clientes',
                route: `/clientes`,
              },
              {
                title: 'Nuevo Cliente',
                route: '/home',
              },
              {
                title: 'Polizas',
                route: `/clientes`,
              },
              {
                title: 'Nueva Poliza',
                route: '/home_p',
              },
        ],
        valid: false,
        formData: {
          name: '',
          email: '',
          password: '',
          rfc: '',
          contacto: '',
          telefono_contacto: '',
          direccion: '',
          rol: ''
        },
      }),
  
      watch: {
        group () {
          this.drawer = false
        },
      },
      created(){
        const id = this.$route.params.id;

        if(id){
          this.loadCliente(id);
        }
      },
      methods: {
        navigateTo(route) {
            this.$router.push(route);
        },
        save() {
            //console.log('Datos guardados: ', this.formData);
            axios.post('cliente/guardar', this.formData)
            .then(responce => {
              this.$refs.form.reset();
              this.formData.id = 0;
              this.$router.push('/clientes');
            }).catch(error => {
              console.error('Ocurrio un error: ', error)
            });
        },
        loadCliente(id){
          axios.get(`cliente/${id}`)
          .then(responce => {
            this.formData = { ...responce.data, password: ''};
          })
          .catch(error => console.error('Error al obtener cliente:', error))
        },
    },
    }
  </script>