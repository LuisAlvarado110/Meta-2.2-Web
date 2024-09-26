<template>
  <v-layout class="rounded rounded-md">
    <v-app-bar>
      <v-spacer></v-spacer> <!-- Empuja el título hacia el centro -->
        <div class="text-center">Meta 2.2 - Consulta y modificación con Vuetify</div>
      <v-spacer></v-spacer> <!-- Empuja el título hacia el centro -->
    </v-app-bar>

    <v-navigation-drawer>
      <v-list>
        <v-list-item prepend-icon="mdi-thumb-up" to="/Galeria" title="Inicio"></v-list-item>
        <v-list-item prepend-icon="mdi-information" to="/pagina2" title="Tarjeta"></v-list-item>
        <v-list-item prepend-icon="mdi-movie-filter" to="/pelis" title="Pelis"></v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main class="d-flex align-center justify-center" style="min-height: 300px;">
      <router-view />
      
    </v-main>
  </v-layout>



  <v-app>
    <v-main>
      <!--<router-view />-->
    </v-main>

    <AppFooter />
  </v-app>
</template>

<script setup>
  //
</script>
