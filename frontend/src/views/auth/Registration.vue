<template>
  <form @submit.prevent="register">
    <div class="form-head">
      <a href="index.html" class="logo"
        ><img
          src="http://themesbox.in/admin-templates/gappa/html/light/assets/images/logo.svg"
          class="img-fluid"
          alt="logo"
      /></a>
    </div>
    <h4 class="text-primary my-4">Create an Account</h4>

    <div class="form-floating mb-3">
      <input
        type="text"
        class="form-control"
        v-model="user.username"
        placeholder="name@example.com"
      />
      <label>Username</label>
    </div>

    <div class="form-floating mb-3">
      <input
        type="text"
        class="form-control"
        v-model="user.first_name"
        placeholder="First Name"
      />
      <label>First Name</label>
    </div>

    <div class="form-floating mb-3">
      <input
        type="text"
        class="form-control"
        v-model="user.last_name"
        placeholder="Last Name"
      />
      <label>Last Name</label>
    </div>

    <div class="form-floating mb-3">
      <input
        type="password"
        autocomplete="false"
        class="form-control"
        v-model="user.password"
        placeholder="Password"
      />
      <label>Password</label>
    </div>

    <div class="d-grid gap-2">
      <button class="btn btn-success btn-lg btn-block font-18" type="submit">
        Sign up
      </button>
    </div>
  </form>

  <p class="mb-0 mt-3">
    Already have an account?
    <router-link to="/login">Log in</router-link>
  </p>
</template>

<script>
import toastr from "toastr";
import axios from "../../axios";

export default {
  name: "Registration",
  data() {
    return {
      user: {
        username: "",
        first_name: "",
        last_name: "",
        password: "",
      },
      error: [],
    };
  },
  methods: {
    async register() {
      await axios
        .post("chat/registration/", this.user)
        .then((response) => {
          console.log(response);
          toastr.success(response.data.message, "Success");
          this.$store.commit("SET_TOKEN", response.data.token);
          this.$store.commit("UPDATE_USER", response.data.user);
          this.$router.replace("/");
        })
        .catch((error) => {
          console.log(error.response);
          this.error = error.response.data;
          for (let err in error.response.data) {
            toastr.error(error.response.data[err], err);
          }
        });
    },
  },
};
</script>

<style scoped></style>
