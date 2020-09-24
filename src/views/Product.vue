<template>
  <div class="row">
    <div class="col-lg-12">
      <div class="card">
        <div class="card-body">
          <h5 class="card-title">Create Product</h5>
          <form v-on:submit.prevent="validForm ? save() : update()">
            <div class="form-group">
              <label for="Description">Description</label>
              <input
                type="text"
                class="form-control"
                id="Description"
                v-model="form.description"
                required
              />
            </div>

            <div class="form-group">
              <label for="Reference">Reference</label>
              <input
                type="text"
                class="form-control"
                id="Reference"
                v-model="form.reference"
                required
              />
            </div>

            <div class="form-group">
              <label for="stock">stock</label>
              <input
                type="number"
                class="form-control"
                id="stock"
                v-model="form.stock"
                required
              />
            </div>

            <div class="form-group">
              <label for="Currency">Currency</label>
              <select class="form-control" v-model="form.currency" required>
                <option value="USD">USD</option>
                <option value="COP">COP</option>
              </select>
            </div>

            <div class="form-group">
              <label for="price">price</label>
              <input
                type="number"
                class="form-control"
                id="price"
                step=".01"
                min="0"
                value="0" 
                v-model="form.price"  
                required           
                
              />
            </div>

            <div class="row">
              <div class="col-md-6">
                <button type="submit" class="btn btn-primary btn-lg btn-block">
                  Send
                </button>
              </div>
              <div class="col-md-6">
                <router-link class="btn btn-secondary btn-lg btn-block" to="/"
                  >Cancel</router-link
                >
              </div>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
//import * as $ from "jquery";

import { http } from "../services";
import swal from "sweetalert";

export default {
  name: "Product",
  data() {
    return {
      loading: false,
      select_info_room: [],
      form: {
        type: "",
        checkin: "",
        checkout: "",
      },
      validForm: true,
    };
  },
  mounted() {
    console.info("App this router:", this.$router);
    console.info("App currentRoute:", this.$router.currentRoute);
    console.info("App currentRoute:", this.$router.currentRoute.name);
    console.info("App paramas:", this.$route.params);

    if (Object.entries(this.$route.params).length > 0) {
      this.validForm = false;
      this.filter(this.$route.params.id);
    }


  },
  methods: {
    filter(dataId) {
      http
        .get(`http://localhost:8000/api/product/${dataId}`, {
          params: { id: dataId },
        })
        .then((response) => {
          console.log(response.data);
          if (response.data.status == true) {
            console.log(response.data);
            this.form = response.data.data[0];
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },

    save() {
      this.loading = true;
      console.log("send request", this.form);

      http
        .post("http://localhost:8000/api/product", this.form)
        .then((response) => {
          console.log(response.data);
          if (response.data.status == true) {
            swal({
              title: "Data sent",
              text: "Your information has been received satisfactorily",
              icon: "success",
            });

            this.$router.push("/");
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },

    update() {
      this.loading = true;
      console.log("send request", this.form);
      this.form.id = this.$route.params.id;

      http
        .put(`http://localhost:8000/api/product/${this.form.id}`, this.form)
        .then((response) => {
          console.log(response.data);
          if (response.data.status == true) {
            swal({
              title: "Data sent",
              text: "Your information has been received satisfactorily",
              icon: "success",
            });

            this.$router.push("/");
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
};
</script>

<style lang="scss">
</style>
