<template>
  <div>
    <Navbar :updateKeranjang="keranjangs" />
    <div class="container">
      <!-- Breadcrumb -->
      <div class="row mt-4">
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
                Keranjang
              </li>
            </ol>
          </nav>
        </div>
      </div>
      <!-- End of breadcrumb -->

      <!-- Start of table -->
      <div class="row">
        <div class="col">
          <h2>Keranjang <strong>Saya</strong></h2>
          <div class="table-responsive mt-3">
            <table class="table">
              <thead>
                <tr>
                  <th scope="col">No</th>
                  <th scope="col">Foto</th>
                  <th scope="col">Makanan</th>
                  <th scope="col">Keterangan</th>
                  <th scope="col">Jumlah</th>
                  <th scope="col">Harga</th>
                  <th scope="col">Total Harga</th>
                  <th scope="col">Hapus</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(keranjang, index) in keranjangs"
                  :key="keranjang.id"
                >
                  <th scope="row">{{ index + 1 }}</th>
                  <td>
                    <img
                      :src="'../assets/img/' + keranjang.product.gambar"
                      class="mb-3 img-fluid shadow rounded"
                      width="250"
                      alt=""
                    />
                  </td>
                  <td>{{ keranjang.product.nama }}</td>
                  <td>
                    {{ keranjang.keterangan ? keranjang.keterangan : '-' }}
                  </td>
                  <td class="text-center">
                    {{ keranjang.jumlah_pesanan }}
                  </td>
                  <td class="text-right">Rp. {{ keranjang.product.harga }}</td>
                  <td class="text-right">
                    <strong>
                      Rp.
                      {{ keranjang.jumlah_pesanan * keranjang.product.harga }}
                    </strong>
                  </td>
                  <td class="text-center text-danger">
                    <b-icon-trash
                      @click="hapusPesanan(keranjang.id)"
                    ></b-icon-trash>
                  </td>
                </tr>

                <tr v-if="keranjangs.length > 0">
                  <td colspan="6" class="text-right">
                    <strong>Total Harga: </strong>
                  </td>
                  <td class="text-right">
                    <strong> Rp. {{ totalHarga }} </strong>
                  </td>
                  <td></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
      <!-- end of table -->

      <!-- form checkout -->
      <div class="row justify-content-end" v-if="keranjangs.length > 0">
        <div class="col-md-4">
          <form class="mt-4" v-on:submit.prevent>
            <div class="form-group">
              <label for="nama">Nama: </label>
              <input
                v-model="pesanan.nama"
                type="text"
                class="form-control"
                id="nama"
              />
            </div>
            <div class="form-group">
              <label for="noMeja">Nomor Meja: </label>
              <input
                v-model="pesanan.noMeja"
                type="text"
                class="form-control"
                id="noMeja"
              />
            </div>
            <button
              type="submit"
              class="btn btn-success float-right"
              @click="checkout"
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
  name: 'Keranjang',
  components: {
    Navbar,
  },

  data() {
    return {
      keranjangs: [],
      pesanan: {},
    };
  },

  methods: {
    setKerangjang: function(data) {
      this.keranjangs = data;
    },

    hapusPesanan: function(id) {
      axios
        .delete('http://localhost:3000/keranjangs/' + id)
        .then((response) => {
          this.$toast.error('Pesanan Berhasil Dihapus!', {
            type: 'error',
            position: 'top-right',
            duration: 2000,
            dismissible: true,
          });

          /* update data pesanan */
          axios
            .get('http://localhost:3000/keranjangs')
            .then((response) => this.setKerangjang(response.data))
            .catch((error) => console.log('Gagal: ', error));
        })
        .catch((error) => console.log('Gagal: ', error));
    },

    checkout: function() {
      if (this.pesanan.nama && this.pesanan.noMeja) {
        this.pesanan.keranjangs = this.keranjangs;
        axios
          .post('http://localhost:3000/pesanans', this.pesanan)
          .then((response) => {
            this.keranjangs.map((item) => {
              axios
                .delete('http://localhost:3000/keranjangs/' + item.id)
                .catch((error) => console.log('Gagal: ', error));
            });

            this.$toast.success('Pesanan Sudah Masuk!', {
              type: 'success',
              position: 'top-right',
              duration: 2000,
              dismissible: true,
            });

            /* Push router to kerangjang */
            this.$router.push({ path: '/pesanan-sukses' });
          })
          .catch((error) => {
            console.log(error);
          });
      } else {
        this.$toast.error('Nama & Nomor Meja Harus Diisi!', {
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
      .get('http://localhost:3000/keranjangs')
      .then((response) => this.setKerangjang(response.data))
      .catch((error) => console.log('Gagal: ', error));
  },

  computed: {
    totalHarga: function() {
      return this.keranjangs.reduce((items, data) => {
        return items + data.product.harga * data.jumlah_pesanan;
      }, 0);
    },
  },
};
</script>

<style></style>
