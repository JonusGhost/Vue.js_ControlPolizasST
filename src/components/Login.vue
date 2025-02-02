<template>
    <v-app>
      <v-container class="d-flex align-center justify-center" style="height: 100vh; background-color: #000;">
        <v-card class="pa-5" elevation="10" max-width="400">
          <v-card-title class="text-h5 text-center">
            Inicia Sesión
          </v-card-title>
          <v-card-text>
            <!-- Mensaje de error -->
            <v-alert
              v-if="errorMessage"
              type="error"
              dismissible
              class="mb-4"
              @input="errorMessage = ''"
            >
              {{ errorMessage }}
            </v-alert>
  
            <!-- Formulario de inicio de sesión -->
            <v-form @submit.prevent="login">
              <!-- Campo de correo electrónico -->
              <v-text-field
                v-model="email"
                label="Usuario"
                outlined
                dense
                required
                class="mb-4"
              ></v-text-field>
  
              <!-- Campo de contraseña -->
              <v-text-field
                v-model="password"
                label="Contraseña"
                type="password"
                outlined
                dense
                required
                class="mb-4"
              ></v-text-field>
  
              <!-- Botón de inicio de sesión -->
              <v-btn
                block
                color="primary"
                class="mt-4"
                type="submit"
                :loading="isLoading"
              >
                Iniciar Sesión
              </v-btn>
            </v-form>
          </v-card-text>
        </v-card>
      </v-container>
    </v-app>
  </template>
  
  <script setup lang="ts">
  import { ref } from "vue";
  import { useRouter } from "vue-router";
  import axios from "../config/axios";
  import { AxiosError } from "axios";
  
  const email = ref("");
  const password = ref("");
  
  const errorMessage = ref("");

  const router = useRouter();
  
  const login = async () => {
    try {
      if (!email.value || !password.value) {
        errorMessage.value = "Por favor, completa todos los campos.";
        return;
      }
  
      const response = await axios.post("/login", {
        email: email.value,
        password: password.value,
      });

      console.log(response.data);

      if (response.data.acceso === "Ok") {
        const token = response.data.token;
        localStorage.setItem("token", token); 

        await router.push({ name: "/home" });
      } else {
        errorMessage.value = response.data.error;
      }
    } catch (err) {
      if (err instanceof AxiosError) {
        if (err.response && err.response.data) {
          errorMessage.value = err.response.data.message || "Error en la solicitud.";
        } else {
          errorMessage.value = "Error de conexión con el servidor.";
        }
      } else {
        errorMessage.value = "Ocurrió un error inesperado. Inténtelo de nuevo.";
      }
    }
  };
  </script>