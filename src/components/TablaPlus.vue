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
  <v-data-table height="600"
    :headers="headers"
    :items="datosPosts"
    :sort-by="[{ key: 'id', order: 'asc' }]"
    class="elevation-1"
  >
    <template v-slot:top>
      <v-toolbar
        flat
      >
        <v-toolbar-title>Lista de Autores y sus Posts</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        <v-dialog
          v-model="dialog"
          max-width="500px"
        >
          <template v-slot:activator="{ props }">
            <v-btn
              color="primary"
              dark
              class="mb-2"
              v-bind="props"
            >
              Nuevo item
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.name"
                      label="Nombre del Autor"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.title"
                      label="Título"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                    <v-text-field
                      v-model="editedItem.body"
                      label="Post"
                    ></v-text-field>
                  </v-col>
                  <v-col
                    cols="12"
                    sm="6"
                    md="4"
                  >
                  </v-col>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="blue-darken-1"
                variant="text"
                @click="close"
              >
                Cancelar
              </v-btn>
              <v-btn
                color="blue-darken-1"
                variant="text"
                @click="save"
              >
                Guardar
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5">¿Estás seguro que lo quieres eliminar?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue-darken-1" variant="text" @click="closeDelete">Cancelar</v-btn>
              <v-btn color="blue-darken-1" variant="text" @click="deleteItemConfirm">OK</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:item.actions="{ item }">
      <v-icon
        size="small"
        class="me-2"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        size="small"
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon>
    </template>
    <template v-slot:no-data>
      <v-btn
        color="primary"
        @click="initialize"
      >
        Reset
      </v-btn>
    </template>
  </v-data-table>
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
data: () => ({
  dialog: false,
  dialogDelete: false,
  headers: [
    {
      title: 'Autor',
      align: 'center',
      width: "100px",
      sortable: false,
      key: 'name',
    },
    { title: 'Título', key: 'title', sortable: false, align: "center", width: "100px" },
    { title: 'Post', key: 'body', sortable: false, align: "center", width: "100px" },
  ],
  datosPosts: [],
  editedIndex: -1,
  editedItem: {
    userId: '',
    title: '',
    body: '',
  },
  defaultItem: {
    userId: '',
    title: '',
    body: '',
  },
  users: [],
}),

computed: {
  formTitle () {
    return this.editedIndex === -1 ? 'Nuevo Item' : 'Editar Item'
  },
},

watch: {
  dialog (val) {
    val || this.close()
  },
  dialogDelete (val) {
    val || this.closeDelete()
  },
},

created () {
  this.initialize()
},

methods: {
  initialize () {
  },

  editItem (item) {
    this.editedIndex = this.datosPosts.indexOf(item)
    this.editedItem = Object.assign({}, item)
    this.dialog = true
  },

  deleteItem (item) {
    this.editedIndex = this.datosPosts.indexOf(item)
    this.editedItem = Object.assign({}, item)
    this.dialogDelete = true
  },

  deleteItemConfirm () {
    this.datosPosts.splice(this.editedIndex, 1)
    this.closeDelete()
  },

  close () {
    this.dialog = false
    this.$nextTick(() => {
      this.editedItem = Object.assign({}, this.defaultItem)
      this.editedIndex = -1
    })
  },

  closeDelete () {
    this.dialogDelete = false
    this.$nextTick(() => {
      this.editedItem = Object.assign({}, this.defaultItem)
      this.editedIndex = -1
    })
  },

  save () {
    if (this.editedIndex > -1) {
      Object.assign(this.datosPosts[this.editedIndex], this.editedItem)
    } else {
      this.datosPosts.push(this.editedItem)
    }
    this.close()
  },
  // Obtiene el nombre del Autor a partir de su userID, recorre la lista hasta encontrar
  // un autor cuyo id coincida con el userId que se proporcionó.
  getUser(userId) {
    const user = this.users.find((user) => user.id === userId);
    return user ? user.name : "";
  },
},

async mounted() {
  try {
    // Se realizan de una dos solicitudes HTTP para sacar los Posts y los Users.
    const [postsResponse, usersResponse] = await Promise.all([
      fetch("https://jsonplaceholder.typicode.com/posts"),
      fetch("https://jsonplaceholder.typicode.com/users"),
    ]);
    // Se obtiene el contenido de las respuestas como objetos JSON con await
    // Se almacenan en datosPosts/users.
    this.datosPosts = await postsResponse.json(); // Posts\Títulos
    this.users = await usersResponse.json(); // Users\Id
    // Se modifican los datos de los posts para agregar el nombre del Autor.
    // Para cada post se crea un nuevo objeto que copia todos los atributos del original,
    // para después agregar la propiedad de Name al objeto, obteniendo su nombre por medio de su ID.
    this.datosPosts = this.datosPosts.map(post => ({
      ...post,
      name: this.getUser(post.userId),
    }));
  } catch (error) {
    console.log(error);
  }
},
};
</script>