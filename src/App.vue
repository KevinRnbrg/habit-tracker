<template>
  <v-app>
    <NavBar />
    <v-main>
      <router-view/>
    </v-main>
    <Footer />
  </v-app>
</template>

<script setup>
import NavBar from './components/NavBar.vue';
import Footer from './components/Footer.vue';

  //
</script>

<style scoped>
  v-main {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
  }
  Footer {
    max-height: 100px;
    margin-top: 40px;
  }
</style>