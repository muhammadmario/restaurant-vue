<template>
  <div>
    <NavbarComponent />
    <div class="container">
      <div class="row">
        <div class="col">
          <h2>Daftar Makanan</h2>
        </div>
      </div>
      <div class="row">
        <div class="col">
          <div class="input-group mb-3">
            <input
              v-model="search"
              @keyup="searchFood"
              type="text"
              class="form-control"
              placeholder="Cari Makanan"
              aria-label="Cari"
              aria-describedby="basic-addon1"
            />
            <div class="input-group-prepend">
              <span class="input-group-text" id="basic-addon1">@</span>
            </div>
          </div>
        </div>
      </div>
      <div class="row">
        <div class="col-md-4" v-for="product in products" :key="product.id">
          <CardProductComponent :product="product" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import NavbarComponent from "@/components/NavbarComponent.vue";
import CardProductComponent from "@/components/CardProductComponent.vue";
import axios from "axios";

export default {
  name: "FoodsView",
  components: {
    NavbarComponent,
    CardProductComponent,
  },
  data() {
    return {
      products: [],
      search: "",
    };
  },
  methods: {
    setProducts(data) {
      this.products = data;
    },
    async searchFood() {
      try {
        const response = await axios.get(
          "http://localhost:3000/products?q=" + this.search
        );
        console.log(response.data);
        this.setProducts(response.data);
      } catch (error) {
        console.error(error);
      }
    },
  },
  async mounted() {
    try {
      const response = await axios.get("http://localhost:3000/products");
      console.log(response.data);
      this.setProducts(response.data);
    } catch (error) {
      console.error(error);
    }
  },
};
</script>

<style></style>
