<template>
  <div>
    <Navbar />
    <div class="container">
      <div class="row mt-4">
        <div class="col">
          <h2>Daftar <strong>Makanan</strong></h2>
        </div>
      </div>

      <div class="row mt-2 mb-3">
        <div
          class="col-md-3 col-sm-6 mt-4 d-flex align-items-stretch"
          v-for="product in products"
          :key="product.id"
        >
          <CardProduct :product="product" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import CardProduct from '@/components/CardProduct.vue';
import Navbar from '@/components/Navbar.vue';
import axios from 'axios';

export default {
  name: 'Foods',
  components: {
    Navbar,
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
      .get('http://localhost:3000/products')
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log('Gagal: ', error));
  },
};
</script>

<style></style>
