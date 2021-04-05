<template>
  <div class="home">
    <Navbar />
    <div class="container">
      <Hero />
      <div class="row mt-5">
        <div class="col">
          <h2>Best <strong>Foods</strong></h2>
        </div>
        <div class="col">
          <router-link to="/foods" class="btn btn-success float-right">
            <b-icon-eye></b-icon-eye>
            Lihat Semua
          </router-link>
        </div>
      </div>

      <div class="row mt-2 mb-3">
        <div
          class="col-md-4 mt-4 d-flex align-items-stretch"
          v-for="product in products"
          :key="product.id"
        >
          <CardProduct :product="product" />
        </div>
      </div>
    </div>
    <Footer />
  </div>
</template>

<script>
// @ is an alias to /src
import CardProduct from '@/components/CardProduct.vue';
import Navbar from '@/components/Navbar.vue';
import Hero from '@/components/Hero.vue';
import axios from 'axios';

export default {
  name: 'Home',

  components: {
    Navbar,
    Hero,
    CardProduct,
  },

  data() {
    return {
      products: [],
    };
  },
  methods: {
    setProduct: function(data) {
      this.products = data;
    },
  },

  mounted() {
    axios
      .get('http://localhost:3000/best-products')
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log('Gagal: ', error));
  },
};
</script>
