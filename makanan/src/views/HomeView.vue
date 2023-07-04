<template>
  <div class="home">
    <NavbarComponent />
    <div class="container">
      <HeroComponent />
      <div class="row">
        <div class="col-md-4" v-for="product in products" :key="product.id">
          <CardProductComponent :product="product" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import NavbarComponent from "@/components/NavbarComponent.vue";
import HeroComponent from "@/components/HeroComponent.vue";
import CardProductComponent from "@/components/CardProductComponent.vue";
import axios from "axios";

export default {
  name: "HomeView",
  components: {
    NavbarComponent,
    HeroComponent,
    CardProductComponent,
  },
  data() {
    return {
      products: [],
      error: false,
    };
  },
  methods: {
    setProducts(data) {
      this.products = data;
    },
    setError(data) {
      this.error = data;
    },
  },
  async mounted() {
    try {
      const response = await axios.get("http://localhost:3000/best-products");
      this.setProducts(response.data);
    } catch (error) {
      this.setError(true);
      console.error(error);
    }
  },
};
</script>
