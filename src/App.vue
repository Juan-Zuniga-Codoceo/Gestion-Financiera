<template>
  <v-app>
    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  data: () => ({
    //
  }),
};
</script>
<style>
/* Establecer el color de fondo en toda la página */
#app {
  background-image: url("@/img/calculadora.jpg"); /* Ruta de la imagen de fondo */
  background-size: cover; /* Ajusta la imagen al tamaño del contenedor */
  background-repeat: no-repeat; /* Evita la repetición de la imagen */
  height: 100vh; /* Asegura que el fondo ocupe la altura completa */
}

</style>