<template>
  <div class="row">
    <div class="col-lg-12">
      <router-link class="btn btn-outline-dark" to="/product">Add</router-link>
      <table class="table table-bordered">
        <thead>
          <tr>
            <th>id</th>
            <th>Description</th>
            <th>Reference</th>
            <th>Stock</th>
            <th>Currency</th>
            <th>Price</th>
            <th>image</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in info_reservation" :key="item.id_reservation">
            <td>{{item.id}}</td>            
            <td>{{item.description}}</td>
            <td>{{item.reference}}</td>
            <td>{{item.stock}}</td>
            <td>{{item.currency}}</td>
            <td>{{item.price}}</td>
            <td> <img :src="item.image" alt="image"> </td>
            <td class="text-center">              
              <router-link class="btn btn-warning btn-sm btn-size" :to="{ name: 'product', params: { id: item.id }}">Edit</router-link>
              <button
                type="button"
                class="btn btn-danger btn-sm btn-size"
                @click="destroy(item.id)"
              >Delete</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import { http } from "../services";
import swal from "sweetalert";

export default {
  name: "Home",
  data() {
    return {
      info_reservation: [],
      loading: false,
    };
  },
  mounted() {
    console.info("App this router:", this.$router);
    console.info("App currentRoute:", this.$router.currentRoute);
    console.info("App currentRoute:", this.$router.currentRoute.name);

    this.load();
  },
  methods: {
    load() {
      http
        .get("http://localhost:8000/api/product")
        .then((response) => {
          console.log(response.data);
          if (response.data.status == false) {
            console.error(response.data);
          }
          this.loading = true;
          this.info_reservation = response.data.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },

    destroy(dataId) {
      console.log("dataId", dataId);
      swal({
        title: "Are you sure?",
        text: "Once deleted, you will not be able to recover!",
        icon: "warning",
        buttons: true,
        dangerMode: true,
      }).then((willDelete) => {
        if (willDelete) {
          swal("record has been deleted!", {
            icon: "success",
          });

          http
            .delete(`http://localhost:8000/api/product/${dataId}`, {
              data: { id: dataId },
            })
            .then((response) => {
              console.log(response.data);
              if (response.data.status == true) {
                this.load();
              }
            })
            .catch((error) => {
              console.log(error);
            });
        } else {
          //swal("Your imaginary file is safe!");
        }
      });
    },
  },
};
</script>

<style lang="scss">
.btn-size {
  width: 90px;
  height: 30px;
}
</style>
