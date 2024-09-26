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
      currentImage: "https://picsum.photos/1200/500", // Imagen inicial
      imageList: [
        "https://picsum.photos/1200/721",
        "https://picsum.photos/1200/722",
        "https://picsum.photos/1200/723",
      ], // Lista de imágenes a cambiar
      imageIndex: 0,
    }),
  
    methods: {
      changeImage() {
        this.loading = true;
  
        // Simula el tiempo de carga (2000ms) y luego cambia la imagen
        setTimeout(() => {
          this.loading = false;
          this.imageIndex = (this.imageIndex + 1) % this.imageList.length;
          this.currentImage = this.imageList[this.imageIndex];
        }, 500);
      },
    },
  };
  </script>
  