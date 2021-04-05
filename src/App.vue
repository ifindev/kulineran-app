<template>
  <div id="app" class="d-flex flex-column min-vh-100">
    <Navbar />
    <router-view />
    <Footer />
  </div>
</template>

<script>
import Footer from '@/components/Footer.vue';
import Navbar from '@/components/Navbar.vue';

export default {
  components: {
    Navbar,
    Footer,
  },
};
</script>
