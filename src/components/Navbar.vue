<template>
  <nav class="navbar navbar-expand-lg navbar-light">
    <div class="container pt-4">
      <a class="navbar-brand" href="#">Kulineran</a>
      <button
        class="navbar-toggler navbar-toggler-right"
        type="button"
        data-toggle="collapse"
        data-target="#navbar"
        aria-controls="navbar"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbar">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item">
            <router-link class="nav-link" to="/">Home</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/foods">Foods</router-link>
          </li>
        </ul>

        <ul class="navbar-nav ml-auto">
          <li class="nav-item">
            <router-link class="nav-link" to="/keranjang">
              Keranjang
              <b-icon-bag></b-icon-bag>
              <span class="badge badge-success ml-2">
                {{
                  updateKeranjang
                    ? updateKeranjang.length
                    : jumlah_pesanan.length
                }}
              </span>
            </router-link>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>

<script>
import axios from 'axios';

export default {
  name: 'Navbar',

  props: ['updateKeranjang'],

  data() {
    return {
      jumlah_pesanan: [],
    };
  },

  methods: {
    setJumlah(data) {
      this.jumlah_pesanan = data;
    },
  },

  mounted() {
    axios
      .get('http://localhost:3000/keranjangs')
      .then((response) => this.setJumlah(response.data))
      .catch((error) => console.log('Gagal: ', error));
  },
};
</script>

<style></style>
