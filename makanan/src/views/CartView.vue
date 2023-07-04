<template>
  <div class="cart-view">
    <NavbarComponent :updateCart="cart" />
    <div class="container">
      <!-- breadcrumb -->
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

      <div class="row">
        <div class="col">
          <h2>
            Keranjang
            <strong>Saya</strong>
          </h2>
          <div class="table-responsive mt-3">
            <table class="table">
              <thead>
                <tr>
                  <th scope="col">#</th>
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
                <tr v-for="product in cart" :key="product.id">
                  <th>1</th>
                  <td>
                    <img
                      :src="'../assets/images/' + product.products.gambar"
                      class="img-fluid shadow"
                      width="250"
                    />
                  </td>
                  <td>
                    <strong>{{ product.products.nama }}</strong>
                  </td>
                  <td>{{ product.keterangan }}</td>
                  <td>{{ product.jumlah_pemesanan }}</td>
                  <td align="right">Rp. {{ product.products.harga }}</td>
                  <td align="right">
                    <strong
                      >Rp.
                      {{ product.jumlah_pemesanan * product.products.harga }}
                    </strong>
                  </td>
                  <td align="center" class="text-danger">
                    <b-icon-trash
                      @click="deleteItem(product.id)"
                    ></b-icon-trash>
                  </td>
                </tr>

                <tr>
                  <td colspan="6" align="right">
                    <strong>Total Harga :</strong>
                  </td>
                  <td align="right">
                    <strong>Rp.{{ totalHarga }}</strong>
                  </td>
                  <td></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
      <!-- Form Checkout -->
      <div class="row justify-content-end">
        <div class="col-md-4">
          <form class="mt-4" v-on:submit.prevent>
            <div class="form-group">
              <label for="nama">Nama :</label>
              <input type="text" class="form-control" v-model="buy.nama" />
            </div>
            <div class="form-group">
              <label for="noMeja">Nomor Meja :</label>
              <input type="text" class="form-control" v-model="buy.noMeja" />
            </div>

            <button
              type="submit"
              class="btn btn-success float-right"
              @click="checkout"
            >
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
  name: "CartView",
  components: {
    NavbarComponent,
  },
  data() {
    return {
      cart: [],
      buy: {},
    };
  },
  methods: {
    setCart(data) {
      this.cart = data;
    },
    async deleteItem(id) {
      try {
        await axios.delete("http://localhost:3000/keranjangs/" + id);
        this.$toast.error("Sukses Hapus Keranjang", {
          type: "error",
          position: "top-right",
          duration: 3000,
          dismissible: true,
        });
        const response = await axios.get("http://localhost:3000/keranjangs");
        await this.setCart(response.data);
      } catch (error) {
        console.log(error);
      }
    },
    async checkout() {
      console.log(this.buy);
      if (this.buy.name || this.buy.noMeja) {
        this.buy.cart = this.cart;
        await axios.post("http://localhost:3000/pesanans", this.buy);
        this.$toast.success("Sukses Dipesan", {
          type: "success",
          position: "top-right",
          duration: 3000,
          dismissible: true,
        });
        await Promise.all(
          this.cart.map(async (item) => {
            try {
              await axios.delete("http://localhost:3000/keranjangs/" + item.id);
            } catch (error) {
              console.log(error);
            }
          })
        );
        await this.$router.push({ path: "/buy-success-message" });
      } else {
        this.$toast.error("Nama dan Nomor Meja Harus diisi", {
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
      const response = await axios.get("http://localhost:3000/keranjangs");
      this.setCart(response.data);
    } catch (error) {
      console.error(error);
    }
  },
  computed: {
    totalHarga() {
      return this.cart.reduce(function (items, data) {
        return items + data.products.harga * data.jumlah_pemesanan;
      }, 0);
    },
  },
};
</script>

<style></style>
