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

      <v-app-bar-title>Home</v-app-bar-title>
    </v-app-bar>

    <v-main>
  <v-card
    class="mx-auto"
    max-width="344"
  >
    <v-img
      :src="url"
      cover
    ></v-img>
    <v-card-actions>
      <v-btn
        color="orange-lighten-2"
        variant="text"
        @click="cambiarImagen"
      >
        Cambiar IMG RANDOM
      </v-btn>
      <v-spacer></v-spacer>
    </v-card-actions>

    <v-expand-transition>
      <div v-show="show">
        <v-divider></v-divider>
      </div>
    </v-expand-transition>
  </v-card>
    </v-main>
  </v-app>
</template>

<script setup>
  import { ref } from 'vue'
  const drawer = ref(null)
  const show = ref(false);
  //show.value = true; // Muestra el texto por default.
  const urlImagen = ref("https://random.imagecdn.app/300/400");
  const url = ref(urlImagen.value);
  
  async function cambiarImagen(){
    // Para acceder al valor es .value si se declara con ref
    const response = await fetch(urlImagen.value,{
        method: "GET",
        headers: {
          "Content-type":"application/json"
        }
    });
    //const data = await response.json();
    console.log(response.url);
    url.value = response.url;
    //console.log("Cambiando Imagen");
    //console.log(response);
  }  
</script>

<script>
  export default {
    data: () => ({ drawer: null }),
  }
</script>
