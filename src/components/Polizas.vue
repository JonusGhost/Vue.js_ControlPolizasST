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
            <v-card>
                <v-card-title class="text-h6">Lista de Polizas</v-card-title>
                    <v-divider></v-divider>
                    <v-card-text>
                        <v-data-table :headers="headers" :items="polizas" item-key="id">
                            <template v-slot:item.actions="{ item }">
                                <v-btn icon @click="editPoliza(item.id)">
                                    <v-icon>mdi-pencil</v-icon>
                                </v-btn>
                                <v-btn icon @click="deletePoliza(item.id)">
                                    <v-icon>mdi-delete</v-icon>
                                </v-btn>
                            </template>
                            <template v-slot:item.alerta="{ item }">
                                <v-chip v-if="alertas.includes(item.id)" color="red" dark>
                                    ⚠️ Proxima a vencer ⚠️
                                </v-chip>
                                <v-chip v-else color="green">
                                    ✅ Vigente
                                </v-chip>
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
    data () {
        return {
            polizas: [],
            alertas: [],
            headers: [
                { title: 'Horas', value: 'total_horas'},
                { title: 'Fecha inicio', value: 'fecha_inicio'},
                { title: 'Fecha fin', value: 'fecha_fin'},
                { title: 'Precio', value: 'precio'},
                { title: 'Cliente', value: 'id_cliente'},
                { title: 'Observaciones', value: 'observaciones'},
                { title: 'Alerta', value: 'alerta', sortable: false},
                { title: 'Acciones', value: 'actions', sortable: false}
            ],
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
        };
    },
    mounted(){
        this.getPolizas();
        this.verificarRenovacion();
    },
    methods: {
      navigateTo(route){
        this.$router.push(route);
      },
      getPolizas(){
          axios.get('polizas')
          .then(response => {
              this.polizas = response.data;
          })
          .catch(error => console.error('Error al obtener polizas:', error));
      },
      verificarRenovacion() {
        axios.get('poliza/verificar-renovacion')
          .then(response => {
            this.alertas = response.data.polizas.map(p => p.id);
          })
          .catch(error => console.error('Error al verificar renovación:', error));
      },
      editPoliza(id) {
          this.$router.push(`/home_p/${id}`);
      },
      deletePoliza(id){
          if(confirm('¿Seguro que deseas eliminar esta poliza?')){
              axios.delete(`poliza/eliminar/${id}`)
              .then(response => {
                  if (response.data === 'Ok'){
                      this.getPolizas();
                  }
              })
              .catch(error => console.error('Error al eliminar poliza:', error))
          }
      }
    }
  }
</script>
