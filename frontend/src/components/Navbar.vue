<template>
  <header class="header header--loggedIn">
    <div class="container">
      <router-link to="/" class="header__logo">
        <img src="../assets/images/study-mate.png" />
        <h1>StudyMate</h1>
      </router-link>
      <form
        @submit.prevent="onSubmit"
        class="header__search"
        method="GET"
        action="#"
      >
        <label>
          <svg
            version="1.1"
            xmlns="http://www.w3.org/2000/svg"
            width="32"
            height="32"
            viewbox="0 0 32 32"
          >
            <title>search</title>
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="16"
              height="16"
              fill="currentColor"
              class="bi bi-search"
              viewBox="0 0 16 16"
            >
              <path
                d="M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001c.03.04.062.078.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1.007 1.007 0 0 0-.115-.1zM12 6.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0z"
              />
            </svg>
          </svg>
          <input name="q" v-model="search" placeholder="Search for posts" />
        </label>
      </form>
      <nav class="header__menu">
        <!-- Logged In -->
        <div>
          <div v-if="$store.state.token !== null" class="menu__container">
            <div class="header__user">
              <div class="dropdown">
                <span
                  id="dropdownMenu2"
                  data-toggle="dropdown"
                  aria-haspopup="true"
                  aria-expanded="false"
                  class="toggal-btn"
                >
                  <div class="avatar avatar--medium active">
                    <img
                      id="profile-img"
                      :src="$store.state.activeUser.avator"
                      class="online"
                      alt=""
                    />
                  </div>
                  <div class="profile-container">
                    <!-- {{ $store.state.activeUser.username }} -->
                    <span class="profie-username">
                      {{ $store.state.activeUser.username }}
                    </span>
                    <span>{{ $store.state.activeUser.name }}</span>
                  </div>
                </span>
                <div class="dropdown-menu" aria-labelledby="dropdownMenu2">
                  <router-link
                    :to="{
                      name: 'profile',
                      params: { uuid: $store.state.activeUser.uuid },
                    }"
                    class="dropdown-item"
                  >
                    <span class="material-symbols-outlined">
                      account_circle
                    </span>
                    Profile
                  </router-link>
                  <a class="dropdown-item" @click="logout()">
                    <span class="material-symbols-outlined"> logout </span>
                    <span style="cursor: pointer"> Logout</span>
                  </a>
                </div>
              </div>
            </div>
            <div class="header__user">
              <router-link to="/chat" type="button" class="icon-button">
                <span class="material-symbols-outlined">chat</span>
                <span class="icon-button__badge">2</span>
              </router-link>
            </div>
            <div class="header__user">
              <button
                type="button"
                class="icon-button"
                id="notification"
                data-toggle="dropdown"
                aria-haspopup="true"
                aria-expanded="false"
              >
                <span class="material-icons">notifications</span>
                <!-- <span class="icon-button__badge">2</span> -->
              </button>
              <div
                class="dropdown-menu notification-menu"
                aria-labelledby="notification"
              >
                <span class="no-data"> No Data Available</span>
              </div>
            </div>
            <div class="header__user">
              <a
                style="cursor: pointer"
                href="http://127.0.0.1:8000/shop/"
                target="_blank"
              >
                <div>
                  <span
                    style="font-size: 4rem; color: #ed8d6a"
                    class="material-symbols-outlined"
                  >
                    workspace_premium
                  </span>
                </div>
              </a>
            </div>
          </div>
          <div v-else>
            <router-link to="login">
              <img src="../assets/images/user.png" alt="profile" />
              <p>Login</p>
            </router-link>
          </div>
        </div>

        <!-- Not Logged In -->

        <div class="dropdown-menu"></div>
      </nav>
    </div>
  </header>

  <!-- another app navbar template -->

  <!-- <nav class="navbar navbar-dark bg-success navbar-expand-sm">
    <div class="container">
      <router-link class="navbar-brand" to="/">Vuex</router-link>
      <div class="collapse navbar-collapse">
        <ul class="navbar-nav">
          <li class="nav-item">
            <router-link class="nav-link" to="/counter"> Counter </router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/employees"
              >Employees</router-link
            >
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/users">User List</router-link>
          </li>
        </ul>
      </div>
    </div>
  </nav> -->
</template>

<script>
import axios from "../axios";
export default {
  name: "NavBar",

  data() {
    return {
      search: "",
    };
  },
  methods: {
    onSubmit() {
      console.log("search text", this.search);
      if (this.search.length > 0) {
        this.$router.push({ path: "/", query: { q: this.search } });
      } else {
        this.$router.push({ path: "/" });
      }
      // this.$store.dispatch("getSearchKeyword", { text: this.search });
    },
    pushDefaultSearch() {
      if (this.$route.query.q) {
        console.log("pushed to ", this.$route.query.q);
        this.search = this.$route.query.q;
      }
    },
    async logout() {
      await axios
        .post("chat/logout/")

        .then((response) => {
          this.$store.dispatch("clearState");
          this.$router.push("/login");
          console.log("response", response);
          window.location.reload();
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
  created() {
    this.pushDefaultSearch();
  },
};
</script>
<style scoped>
.notification-menu {
  left: -96px !important;
  width: 98% !important;
  min-height: 95% !important;
  height: 20rem !important;
}
.no-data {
  font-size: 19px;
  padding: 39px;
  display: flex;
  margin: 0 auto;
}
.toggal-btn {
  display: flex;
  cursor: pointer;
}
.profile-container {
  display: flex;
  flex-direction: column;
  margin-left: 17px;
}
.profie-username {
  position: relative;
  right: 9px;
}
</style>
