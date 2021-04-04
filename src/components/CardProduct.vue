<template>
  <div class="card shadow card-product mt-3">
    <img
      class="card-img-top"
      :src="'assets/img/' + product.gambar"
      alt="Gambar Product"
    />
    <div class="card-body d-flex flex-column">
      <h5 class="card-title">{{ product.nama }}</h5>
      <p class="card-text">Harga: Rp. {{ product.harga }}</p>
      <div class="mt-auto">
        <a href="#" class="mt-auto btn btn-success">
          <b-icon-cart></b-icon-cart>
          Pesan
        </a>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CardProduct',
  props: ['product'],
};
</script>

<style></style>
