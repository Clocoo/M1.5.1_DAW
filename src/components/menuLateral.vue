<template>
    <v-app id="inspire">
      <v-navigation-drawer v-model="drawer">
        <!--  --> <v-list dense>
          <v-list>
                    <v-list-subheader>Menu</v-list-subheader>
          </v-list>
            <router-link to="/" tag="li" exact>
                <v-list>
                    <v-list-item prepend-icon="mdi-home">Home</v-list-item>
                </v-list>
    </router-link>
    <router-link to="/tabla" tag="li" exact>
      <v-list>
      <v-list-item prepend-icon="mdi-account">Tabla</v-list-item>
                </v-list>
    </router-link>
      </v-list>
      </v-navigation-drawer>
  
      <v-app-bar>
        <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
  
        <v-app-bar-title></v-app-bar-title>
      </v-app-bar>
  
      <v-main>
        <!--  -->
      </v-main>
    </v-app>
  </template>
  
  <script setup>
    import { ref } from 'vue'
  
    const drawer = ref(null)
  </script>
  
  <script>
    export default {
      data: () => ({ drawer: null }),
    }
  </script>