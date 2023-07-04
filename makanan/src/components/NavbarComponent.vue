<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container">
      <a class="navbar-brand" href="#">Kulineran</a>
      <button
        class="navbar-toggler"
        type="button"
        data-toggle="collapse"
        data-target="#navbarNav"
        aria-controls="navbarNav"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item">
            <router-link class="nav-link" to="/">Home</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/foods">Foods</router-link>
          </li>
        </ul>
        <ul class="navbar-nav ml-auto">
          <li class="nav-item d-flex justify-content-center align-items-center">
            <router-link class="nav-link" to="/cart">Cart</router-link>
            <b-icon-bag></b-icon-bag>
            <span class="badge badge-success ml-2">{{
              updateCart ? updateCart.length : products.length
            }}</span>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>

<script>
import axios from "axios";

export default {
  name: "NavbarComponent",
  data() {
    return {
      products: [],
    };
  },
  props: ["updateCart"],
  methods: {
    setProducts(data) {
      this.products = data;
    },
  },
  async mounted() {
    try {
      const response = await axios.get("http://localhost:3000/keranjangs");
      this.setProducts(response.data);
    } catch (error) {
      console.error(error);
    }
  },
};
</script>

<style></style>
