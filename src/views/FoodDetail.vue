<template>
  <div>
    <Navbar />
    <div class="container">
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
          <form class="mt-4" v-on:submit.prevent>
            <div class="form-group">
              <label for="jumlah_pesanan">Jumlah Pesanan</label>
              <input
                type="number"
                class="form-control"
                id="jumlah_pesanan"
                v-model="pesanan.jumlah_pesanan"
              />
            </div>
            <div class="form-group">
              <label for="keterangan">Keterangan Tambahan</label>
              <textarea
                v-model="pesanan.keterangan"
                type="text"
                class="form-control"
                id="keterangan"
                placeholder="Keterangan seperti: Pedes, Nasi Setengah ..."
              >
              </textarea>
            </div>

            <button
              type="submit"
              class="btn btn-success"
              @click="tambahPesanan"
            >
              <b-icon-cart></b-icon-cart>
              Pesan
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from '@/components/Navbar.vue';
import axios from 'axios';

export default {
  name: 'FoodDetail',
  components: {
    Navbar,
  },
  data() {
    return {
      product: [],
      pesanan: {
        product: [],
        jumlah_pesanan: '',
        keterangan: '',
      },
    };
  },

  methods: {
    setProduct: function(data) {
      this.product = data;
    },
    tambahPesanan: function() {
      this.pesanan.product = this.product;
      if (this.pesanan.jumlah_pesanan) {
        axios
          .post('http://localhost:3000/keranjangs', this.pesanan)
          .then((response) => {
            this.$toast.success('Sukses Masuk Keranjang!', {
              type: 'success',
              position: 'top-right',
              duration: 2000,
              dismissible: true,
            });

            /* Push router to kerangjang */
            this.$router.push({ path: '/keranjang' });
          })
          .catch((error) => {
            console.log(error);
          });
      } else {
        this.$toast.error('Jumlah Pesanan Harus Diisi!', {
          type: 'error',
          position: 'top-right',
          duration: 2000,
          dismissible: true,
        });
      }
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
