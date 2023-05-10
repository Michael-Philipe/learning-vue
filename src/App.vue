<template>
  <NavBar :logo="data.logo_src" :alt="data.app_name" />
  <router-view />
  <Footer />
</template>
<script setup>
import NavBar from '@/components/NavBar.vue';
import Footer from '@/components/Footer.vue';
import { ref } from 'vue';

const data = ref({
  logo_src: '/img/logo.png',
  app_name: 'Make your burguers',
});
</script>

<style>
* {
  font-family: Helvetica;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

#app {
  position: relative;
  min-height: 100vh;
}

.main-container {
  padding: 50px 50px 200px;
  min-height: 250px;
}

h1 {
  text-align: center;
  font-size: 42px;
  margin-bottom: 30px;
  color: #222;
}
</style>
