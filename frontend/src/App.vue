<template>
  <nav-bar
    v-if="
      this.$route.path !== '/chat' &&
      this.$route.name !== 'callerView' &&
      this.$route.name !== 'receiverView'
    "
  />
  <router-view></router-view>
</template>
<script>
import NavBar from "./components/Navbar.vue";
export default {
  components: {
    "nav-bar": NavBar,
  },
  watch: {
    $route: function () {
      console.log("router from app.vue", this.$route);
    },
  },
};
</script>
