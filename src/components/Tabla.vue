<template>
    <v-app id="inspire">
      <v-navigation-drawer v-model="drawer">
        <v-list dense>
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
        <v-app-bar-title>Tabla</v-app-bar-title>
      </v-app-bar>
      <v-main>
        <v-container>
        <v-table height="500" width="700">
          <thead>
            <tr>
              <th class="text-left">
                Autor
              </th>
              <th class="text-left">
                Título
              </th>
              <th class="text-left">
                Post
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in datosPosts" :key="item.id">
              <td>{{ getUser(item.userId) }}</td>
              <td>{{ item.title }}</td>
              <td>{{ item.body }}</td>
            </tr>
          </tbody>
        </v-table>
      </v-container>
      </v-main>
    </v-app>
  </template>
  
  <script setup>
    import { ref } from 'vue'
    const drawer = ref(null)
  </script>

  <script>
  export default {
    data() {
      return {
        datosPosts: [],
        users: [],
      };
    },
    // Obtenemos los Autores, sus Títulos y sus Posts
    async mounted() {
      try {
        const [postsResponse, usersResponse] = await Promise.all([
          fetch("https://jsonplaceholder.typicode.com/posts"),
          fetch("https://jsonplaceholder.typicode.com/users"),
        ]);
        this.datosPosts = await postsResponse.json();
        this.users = await usersResponse.json();
      } catch (error) {
        console.log(error);
      }
    },
    // Guardamos al Autor (Usuario) por medio de su ID de la página de Usuarios.
    methods: {
      getUser(userId) {
        const user = this.users.find((user) => user.id === userId);
        return user ? user.name : "";
      },
    },
  };
  </script>