<template>
  <v-container width="1200px">
    <!-- Mostrar <v-alert> en caso de error -->
    <v-alert
      v-if="error"
      type="error"
      prominent
      border="center"
      color="red"
      class="my-4"
    >
      Error al obtener datos de la tabla
    </v-alert>

    <!-- Mostrar <v-progress-circular> si loading está activo y no hay error -->
    <v-progress-circular
      v-if="loading && !error"
      indeterminate
      color="white"
      size="50"
      class="mx-auto my-5"
    >
    </v-progress-circular>

    <!-- Mostrar la tabla solo cuando los datos ya se han cargado y no hay error -->
    <v-table v-else-if="!loading && !error">
      <thead>
        <tr>
          <th>Título</th>
          <th>Calificación</th>
          <th>IMDb</th>
          <th>Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in movies" :key="item.id">
          <td>{{ item.movie }}</td>
          <td>{{ item.rating }}</td>
          <td>
            <a :href="item.imdb_url" target="_blank">{{ item.imdb_url }}</a>
          </td>
          <td>
            <v-btn @click="openEditDialog(item)">
              <v-icon>mdi-pencil</v-icon>
            </v-btn>
            <v-btn  @click="openDeleteDialog(item)">
              <v-icon>mdi-delete</v-icon>
            </v-btn>
          </td>
        </tr>
      </tbody>
    </v-table>

    <!-- Mostrar <v-progress-linear> mientras los datos se cargan y no hay error -->
    <v-progress-linear
      v-if="loading && !error"
      indeterminate
      color="white"
      class="my-4"
    ></v-progress-linear>

    <!-- Diálogo de edición -->
    <v-dialog v-model="editDialog" persistent max-width="400">
      <v-card>
        <v-card-title>
          <span class="text-h6">Editar Película</span>
        </v-card-title>
        <v-card-text>
          <v-form ref="editForm">
            <v-text-field
              v-model="editedMovie.id"
              label="Id"
              readonly
            ></v-text-field>
            <v-text-field
              v-model="editedMovie.movie"
              label="Movie"
            ></v-text-field>
            <v-text-field
              v-model="editedMovie.rating"
              label="Rating"
            ></v-text-field>
            <v-text-field
              v-model="editedMovie.imdb_url"
              label="IMDB URL"
            ></v-text-field>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text @click="closeEditDialog">Cancelar</v-btn>
          <v-btn color="primary" text @click="saveMovie">Guardar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <!-- Diálogo de eliminación -->
    <v-dialog v-model="deleteDialog" max-width="400">
      <v-card>
        <v-card-title class="text-h6">¿Borrar?</v-card-title>
        <v-card-text>
          Borrar datos de la película: <strong>{{ movieToDelete?.movie }}</strong>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text @click="closeDeleteDialog">Cancelar</v-btn>
          <v-btn color="red" text @click="confirmDeleteMovie">Borrar</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
import { ref, onMounted } from 'vue';

export default {
  setup() {
    const movies = ref([]);
    const loading = ref(true);
    const error = ref(null);

    // Variables del diálogo de edición
    const editDialog = ref(false);
    const editedMovie = ref({
      id: '',
      movie: '',
      rating: '',
      imdb_url: '',
    });

    // Variables del diálogo de eliminación
    const deleteDialog = ref(false);
    const movieToDelete = ref(null);

    const fetchMovies = async () => {
      try {
        const response = await fetch('https://dummyapi.online/api/movies');
        if (!response.ok) {
          throw new Error('Error al obtener los datos del servidor');
        }
        const data = await response.json();
        movies.value = data;
      } catch (err) {
        error.value = err.message;
      } finally {
        loading.value = false;
      }
    };

    const openEditDialog = (movie) => {
      editedMovie.value = { ...movie };
      editDialog.value = true;
    };

    const closeEditDialog = () => {
      editDialog.value = false;
    };

    const saveMovie = () => {
      const index = movies.value.findIndex((m) => m.id === editedMovie.value.id);
      if (index !== -1) {
        movies.value[index] = { ...editedMovie.value };
      }
      closeEditDialog();
    };

    const openDeleteDialog = (movie) => {
      movieToDelete.value = movie;
      deleteDialog.value = true;
    };

    const closeDeleteDialog = () => {
      deleteDialog.value = false;
      movieToDelete.value = null;
    };

    const confirmDeleteMovie = () => {
      movies.value = movies.value.filter((movie) => movie.id !== movieToDelete.value.id);
      closeDeleteDialog();
    };

    onMounted(fetchMovies);

    return {
      movies,
      loading,
      error,
      editDialog,
      editedMovie,
      deleteDialog,
      movieToDelete,
      openEditDialog,
      closeEditDialog,
      saveMovie,
      openDeleteDialog,
      closeDeleteDialog,
      confirmDeleteMovie,
    };
  },
};
</script>
