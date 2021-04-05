<template>
  <div class="container">
    <!-- <h1>Food Detail {{ $route.params.id }}</h1> -->

    <!-- Breadcrumb -->
    <div class="row mt-5">
      <div class="col">
        <nav aria-label="breadcrumb">
          <ol class="breadcrumb">
            <li class="breadcrumb-item">
              <router-link to="/" class="text-dark">Home</router-link>
            </li>
            <li class="breadcrumb-item">
              <router-link to="/foods" class="text-dark">Foods</router-link>
            </li>
            <li class="breadcrumb-item active" aria-current="page">
              Food Order
            </li>
          </ol>
        </nav>
      </div>
    </div>

    <!-- food detail -->
    <div class="row mt-4">
      <div class="col-md-6">
        <img
          :src="'../assets/img/' + product.gambar"
          class="mb-3 img-fluid shadow rounded"
          alt=""
        />
      </div>
      <div class="col-md-6">
        <h2>
          <strong>{{ product.nama }}</strong>
        </h2>
        <hr />
        <h4>
          Harga: <strong>{{ product.harga }}</strong>
        </h4>
        <form class="mt-4">
          <div class="form-group">
            <label for="jumlah_pesanan">Jumlah Pesanan</label>
            <input type="number" class="form-control" id="jumlah_pesanan" />
          </div>
          <div class="form-group">
            <label for="keterangan">Keterangan Tambahan</label>
            <textarea
              type="text"
              class="form-control"
              id="keterangan"
              placeholder="Keterangan seperti: Pedes, Nasi Setengah ..."
            >
            </textarea>
          </div>

          <button type="submit" class="btn btn-success">
            <b-icon-cart></b-icon-cart>
            Pesan
          </button>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'FoodDetail',
  data() {
    return {
      product: [],
    };
  },

  methods: {
    setProduct: function(data) {
      this.product = data;
    },
  },

  mounted() {
    axios
      .get('http://localhost:3000/products/' + this.$route.params.id)
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log('Gagal: ', error));
  },
};
</script>

<style></style>
