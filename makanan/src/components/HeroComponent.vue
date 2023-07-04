<template>
  <div class="row d-flex">
    <div class="col-md-6 order-md-2">
      <img src="../assets/images/ilustrasi.png" alt="hero" width="100%" />
    </div>

    <div class="col-md-6 order-md-1">
      <div class="h-100 d-flex flex-column justify-content-center">
        <h2>
          <strong>Delicious Menu, </strong><br />
          in Your gadget
        </h2>
        <p>Ayo segera pilih dan pesan makanan favorit anda</p>
        <router-link to="/foods" class="btn btn-success btn-lg align-self-start"
          >Pesan</router-link
        >
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HeroComponent",
};
</script>

<style></style>
