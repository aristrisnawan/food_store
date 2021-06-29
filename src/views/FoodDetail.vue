<template>
  <div class="food-detail">
    <Navbar />
    <div class="container">
      <!-- Breadcumb -->
      <div class="row">
        <div class="col mt-5">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/" class="text-dark">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods" class="text-dark">Food</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">Food Order</li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row mt-2">
        <div class="col-md-6 mb-4">
          <img
            :src="'../assets/image/' + product.gambar"
            class="img-fluid shadow"
            alt=""
          />
        </div>
        <div class="col-md-6">
          <h2>
            <strong> {{ product.nama }} </strong>
          </h2>
          <br />
          <h3>
            Harga : <strong>Rp {{ product.harga }}</strong>
          </h3>
          <form v-on:submit.prevent>
            <div class="form-group">
              <label for="jumlah_pemesanan">Jumlah Pesan :</label>
              <input
                type="number"
                class="form-control"
                v-model="pesanan.jumlah_pemesanan"
              />
            </div>
            <div class="form-group">
              <label for="keterangan">Keterangan</label>
              <textarea
                rows=""
                class="form-control"
                placeholder="Keterangan misal:(pedas,manis)"
                cols=""
                v-model="pesanan.keterangan"
              ></textarea>
            </div>
            <button type="submit" class="btn btn-success" @click="pemesanan">
              <b-icon-cart4></b-icon-cart4> Order
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "FoodDetail",
  components: {
    Navbar,
  },
  data() {
    return {
      product: [],
      pesanan: {},
    };
  },
  methods: {
    setProduct(data) {
      this.product = data;
    },
    pemesanan() {
      if (this.pesanan.jumlah_pemesanan) {
        this.pesanan.products = this.product;
        axios
          .post("http://localhost:3000/keranjang", this.pesanan)
          .then(() => {
            this.$router.push({ path: "/keranjang" });
            this.$toast.success("Order Success.", {
              type: "success",
              position: "top-right",
              duration: 3000,
              dismissible: true,
            });
          })
          .catch(() => {
            console.log("gagal");
          });
      } else {
        this.$toast.error("Jumlah pesanan harus diisi.", {
          type: "error",
          position: "top-right",
          duration: 3000,
          dismissible: true,
        });
      }
      // console.log(this.pesanan);
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/product/" + this.$route.params.id)
      .then((response) => this.setProduct(response.data))
      .catch((error) => console.log("Gagal : ", error));
  },
};
</script>

<style></style>
