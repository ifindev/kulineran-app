<template>
  <div>
    <div class="container">
      <div class="row mt-4">
        <div class="col">
          <h2>Daftar <strong>Makanan</strong></h2>
        </div>
      </div>

      <div class="row mt-4">
        <div class="col">
          <div class="input-group mb-3">
            <input
              v-model="search"
              type="text"
              class="form-control p-4"
              placeholder="Cari Makanan Kesukaan Anda ..."
              aria-label="Cari Makanan Kesukaan Anda ..."
              aria-describedby="basic-addon1"
              @keyup="searchFood"
            />
            <div class="input-group-prepend">
              <span class="input-group-text" id="basic-addon1">
                <b-icon-search></b-icon-search>
              </span>
            </div>
          </div>
        </div>
      </div>

      <div class="row mb-3">
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
import axios from 'axios';

export default {
  name: 'Foods',
  components: {
    CardProduct,
  },
  data() {
    return {
      products: [],
      search: '',
    };
  },

  methods: {
    setProduct: function(data) {
      this.products = data;
    },
    searchFood: function(data) {
      axios
        .get('http://localhost:3000/products?q=' + this.search)
        .then((response) => this.setProduct(response.data))
        .catch((error) => console.log('Gagal: ', error));
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
