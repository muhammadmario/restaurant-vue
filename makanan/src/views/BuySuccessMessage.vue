<template>
  <div class="pesanan-sukses">
    <NavbarComponent />
    <div class="container">
      <div class="row justify-content-center mt-5">
        <div class="col text-center">
          <img src="../assets/images/menunggu.png" width="300" />
          <h2><strong>Sukses!!</strong></h2>
          <h4>
            Pesanan Anda segera diproses <br />
            Selamat Menunggu
          </h4>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import NavbarComponent from "@/components/NavbarComponent.vue";

export default {
  name: "BuySuccessMessage",
  components: {
    NavbarComponent,
  },
};
</script>

<style></style>
