<template>
  <div class="keranjang">
    <Navbar :updateKeranjnag="keranjangs" />
    <div class="container">
      <!-- Breadcumb -->
      <div class="row">
        <div class="col mt-4">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/" class="text-dark">Home</router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods" class="text-dark">Food</router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">Keranjang</li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row">
        <div class="col ml-3">
          <h2>Keranjang <strong>Saya</strong></h2>
          <div class="table-responsive mt-3">
            <table class="table text-center">
              <thead>
                <tr>
                  <th scope="col">No</th>
                  <th scope="col">Foto</th>
                  <th scope="col">Makanan</th>
                  <th scope="col">Keterangan</th>
                  <th scope="col">Jumlah</th>
                  <th scope="col">Harga</th>
                  <th scope="col">Total Harga</th>
                  <th scope="col">Action</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(keranjang, index) in keranjangs" :key="keranjang.id">
                  <th scope="row">{{ index + 1 }}</th>
                  <td>
                    <img
                      :src="'../assets/image/' + keranjang.products.gambar"
                      class="img-fluid shadow"
                      width="100px"
                      alt=""
                    />
                  </td>
                  <td>{{ keranjang.products.nama }}</td>
                  <td>
                    {{ keranjang.keterangan ? keranjang.keterangan : "-" }}
                  </td>
                  <td>{{ keranjang.jumlah_pemesanan }}</td>
                  <td>Rp. {{ keranjang.products.harga }}</td>
                  <td>
                    Rp.
                    {{ keranjang.products.harga * keranjang.jumlah_pemesanan }}
                  </td>
                  <td>
                    <button
                      class="btn-md btn-danger"
                      @click="hapusKeranjnag(keranjang.id)"
                    >
                      <b-icon-trash></b-icon-trash>
                    </button>
                  </td>
                </tr>
                <tr>
                  <td colspan="7">
                    <strong>Total Bayar :</strong>
                  </td>
                  <td>
                    <strong>Rp. {{ totalHarga }} </strong>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>

      <!-- form checkout -->
      <div class="row justify-content-end">
        <div class="col-md-">
          <form v-on:submit.prevent>
            <div class="form-group">
              <label for="nama">Nama :</label>
              <input type="text" class="form-control" v-model="pesanan.nama" />
            </div>
            <div class="form-group">
              <label for="nomor_meja">Nomor Meja :</label>
              <input type="text" class="form-control" v-model="pesanan.nomor_meja" />
            </div>
            <button type="submit" class="btn btn-success float-right" @click="checkout">
              <b-icon-cart4></b-icon-cart4> Order
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "../components/Navbar.vue";
import axios from "axios";

export default {
  name: "Keranjang",
  components: {
    Navbar,
  },
  data() {
    return {
      keranjangs: [],
      pesanan: {},
    };
  },
  methods: {
    setKeranjangs(data) {
      this.keranjangs = data;
    },
    hapusKeranjnag(id) {
      axios
        .delete("http://localhost:3000/keranjang/" + id)
        .then(() => {
          this.$toast.success("Data berhasil di hapus.", {
            type: "success",
            position: "top-right",
            duration: 3000,
            dismissible: true,
          });
          //Update data keranjnag && menghilangkan reload
          axios
            .get("http://localhost:3000/keranjang")
            .then((response) => this.setKeranjangs(response.data))
            .catch((error) => console.log("Gagal : ", error));
        })
        .catch((error) => console.log("Gagal : ", error));
    },
    checkout() {
      if (this.pesanan.nama && this.pesanan.nomor_meja) {
        this.pesanan.keranjangs = this.keranjangs;
        axios.post("http://localhost:3000/pesanan", this.pesanan).then(() => {
          this.keranjangs.map(function (item) {
            // Hapus semua keranjang
            return axios
              .delete("http://localhost:3000/keranjang/" + item.id)
              .catch((error) => console.log(error));
          });
          this.$toast.success("Makanan berhasil di order.", {
            type: "success",
            position: "top-right",
            duration: 3000,
            dismissible: true,
          });
          //Update data keranjnag && menghilangkan reload
          axios
            .get("http://localhost:3000/keranjang")
            .then((response) => this.setKeranjangs(response.data))
            .catch((error) => console.log("Gagal : ", error));
        });
      } else {
        this.$toast.error("Nama dan nomor meja harus diisi", {
          type: "error",
          position: "top-right",
          duration: 3000,
          dismissible: true,
        });
      }
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/keranjang")
      .then((response) => this.setKeranjangs(response.data))
      .catch((error) => console.log("Gagal : ", error));
  },
  computed: {
    totalHarga() {
      return this.keranjangs.reduce(function (item, data) {
        return item + data.products.harga * data.jumlah_pemesanan;
      }, 0);
    },
  },
};
</script>

<style></style>
