<template>
  <!-- for start backend 
json-server --watch db.json -->

  <div>
    <b-navbar toggleable="lg" type="light">
      <div class="container">
        <router-link class="navbar-brand" to="/">TuangRaos</router-link>

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <b-navbar-nav>
            <li class="nav-item">
              <router-link class="nav-link" to="/">Home</router-link>
            </li>
            <li class="nav-item">
              <router-link class="nav-link" to="/foods">Foods</router-link>
            </li>
          </b-navbar-nav>

          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto">
            <li class="nav-item">
              <router-link class="nav-link" to="/keranjang"
                >Keranjang
                <b-icon-cart4></b-icon-cart4>
                <span class="badge badge-success ml-2">{{
                  updateKeranjang ? updateKeranjang.length : jumlah_pesanan.length
                }}</span>
              </router-link>
            </li>
          </b-navbar-nav>
        </b-collapse>
      </div>
    </b-navbar>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Navbar",
  data() {
    return {
      jumlah_pesanan: [],
    };
  },
  props: ["updateKeranjang"],
  methods: {
    setJumlah(data) {
      this.jumlah_pesanan = data;
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/keranjang")
      .then((response) => this.setJumlah(response.data))
      .catch((error) => console.log("Gagal : ", error));
  },
};
</script>

<style></style>
