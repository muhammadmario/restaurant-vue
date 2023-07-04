<template>
  <div class="food-detail">
    <NavbarComponent />
    <div class="container">
      <div class="row">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods">Foods</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/order">Order</router-link>
              </li>
            </ol>
          </nav>
        </div>
      </div>
      <div class="row">
        <div class="col-md-6">
          <img
            :src="'../assets/images/' + product.gambar"
            class="img-fluid shadow"
          />
        </div>
        <div class="col-md-6">
          <h2>
            <strong>{{ product.nama }}</strong>
          </h2>
          <hr />
          <h4>
            Harga :
            <strong>Rp. {{ product.harga }}</strong>
          </h4>
          <form class="mt-4" v-on:submit.prevent>
            <div class="form-group">
              <label for="jumlah_pemesanan">Jumlah Pesan</label>
              <input
                type="number"
                class="form-control"
                v-model="pesan.jumlah_pemesanan"
              />
            </div>
            <div class="form-group">
              <label for="keterangan">Keterangan</label>
              <textarea
                v-model="pesan.keterangan"
                class="form-control"
                placeholder="Keterangan spt : Pedes, Nasi Setengah .."
              ></textarea>
            </div>

            <button type="submit" class="btn btn-success" @click="pemesanan">
              <b-icon-cart></b-icon-cart>Pesan
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import NavbarComponent from "@/components/NavbarComponent.vue";
import axios from "axios";

export default {
  name: "FoodDetailView",
  components: {
    NavbarComponent,
  },
  data() {
    return {
      product: [],
      pesan: {},
    };
  },
  methods: {
    setProduct(data) {
      this.product = data;
    },
    async pemesanan() {
      if (this.pesan.jumlah_pemesanan) {
        this.pesan.products = this.product;
        try {
          axios.post("http://localhost:3000/keranjangs", this.pesan);
          this.$router.push({ path: "/cart" });
          this.$toast.success("Sukses Masuk Keranjang", {
            type: "success",
            position: "top-right",
            duration: 3000,
            dismissible: true,
          });
        } catch (error) {
          console.error(error);
        }
      } else {
        this.$toast.error("Jumlah Pesanan Harus diisi", {
          type: "error",
          position: "top-right",
          duration: 3000,
          dismissible: true,
        });
      }
    },
  },
  async mounted() {
    try {
      const response = await axios.get(
        "http://localhost:3000/products/" + this.$route.params.id
      );
      console.log(response.data);
      this.setProduct(response.data);
    } catch (error) {
      console.error(error);
    }
  },
};
</script>

<style></style>
