<template>
  <div class="d-flex flex-column align-center bg-black-lighten-4">
    <div class="center">
      <v-img
        :aspect-ratio="16/9"
          class="bg-black"
          :src="currentImage"
          width="1200"
          height="600"
      ></v-img>
    </div>

    <v-btn
      :loading="loading"
      @click="changeImage"
    >
      Siguiente
      <template v-slot:loader>
        <v-progress-linear indeterminate></v-progress-linear>
      </template>
    </v-btn>

  </div>
  
    
</template>
  
<script>
export default {
  data: () => ({
    loading: false,
    currentImage: `https://picsum.photos/id/${Math.floor(Math.random()*1000)}/1200/720`, // Imagen inicial
  }),

  methods: {
    changeImage() {
      this.loading = true;

      // Simula el tiempo de carga (2000ms) y luego cambia la imagen
      setTimeout(() => {
        this.loading = false;
        this.currentImage = `https://picsum.photos/id/${Math.floor(Math.random()*1000)}/1200/720`; // Cambia la imagen aquí
      }, 500);
    },
  },
};
</script>
  