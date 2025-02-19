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
            <v-card>
                <v-card-title class="text-h6">Lista de Clientes</v-card-title>
                    <v-divider></v-divider>
                    <v-card-text>
                        <v-data-table :headers="headers" :items="clientes" item-key="id">
                            <template v-slot:item.actions="{ item }">
                                <v-btn icon @click="editCliente(item.id)">
                                    <v-icon>mdi-pencil</v-icon>
                                </v-btn>
                                <v-btn icon @click="deleteCliente(item.id)">
                                    <v-icon>mdi-delete</v-icon>
                                </v-btn>
                            </template>
                        </v-data-table>
                    </v-card-text>
            </v-card>
         </v-main>
      </v-layout>
    </v-card>
</template>
<script>
import axios from '../config/axios';

export default {
    data() {
        return {
            clientes: [],
            headers: [
                { title: 'Nombre', value: 'name'},
                { title: 'Correo', value: 'email'},
                { title: 'RFC', value: 'rfc'},
                { title: 'Contacto', value: 'contacto'},
                { title: 'Teléfono', value: 'telefono_contacto'},
                { title: 'Acciones', value: 'actions', sortable: false}
            ],
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
        };
    },
    mounted(){
        this.getClientes();
    },
    methods: {
        getClientes(){
            axios.get('clientes')
            .then(response => {
                this.clientes = response.data;
            })
            .catch(error => console.error('Error al obtener clientes:', error));
        },
        editCliente(id) {
            this.$router.push(`/home/${id}`);
        },
        deleteCliente(id){
            if(confirm('¿Seguro que deseas eliminar este cliente?')){
                axios.delete(`cliente/eliminar/${id}`)
                .then(response => {
                    if (response.data === 'Ok'){
                        this.getClientes();
                    }
                })
                .catch(error => console.error('Error al eliminar cliente:', error))
            }
        }
    }
};
</script>