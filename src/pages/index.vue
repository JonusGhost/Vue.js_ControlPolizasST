<template>
  <v-app>
    <v-container class="d-flex align-center justify-center" style="height: 100vh; background-color: #f5f5f5;">
      <v-card class="pa-5" elevation="10" max-width="400">
        <v-card-title class="text-h5 text-center">
          Inicia Sesión
        </v-card-title>
        <v-card-text>
          <v-form>
            <!-- Campo de Usuario -->
            <v-text-field
              v-model="username"
              label="Usuario"
              outlined
              dense
              required
              class="mb-4"
            ></v-text-field>

            <!-- Campo de Contraseña -->
            <v-text-field
              v-model="password"
              label="Contraseña"
              type="password"
              outlined
              dense
              required
              class="mb-4"
            ></v-text-field>

            <!-- Botón de Iniciar Sesión -->
            <v-btn
              block
              color="primary"
              class="mt-4"
              @click="login"
            >
              Iniciar Sesión
            </v-btn>
          </v-form>
        </v-card-text>
      </v-card>
    </v-container>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      username: "",
      password: "",
    };
  },
  methods: {
    login() {
      // Aquí puedes agregar la lógica para el inicio de sesión
      if (this.username && this.password) {
        alert(`Usuario: ${this.username}\nContraseña: ${this.password}`);
      } else {
        alert("Por favor, completa todos los campos.");
      }
    },
  },
};
</script>

<style>
/* Opcional: Puedes personalizar el diseño aquí */
</style>
