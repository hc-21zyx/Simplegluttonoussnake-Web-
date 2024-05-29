<template>
  <NavBar />
  <router-view></router-view>
</template>

<script>
import NavBar from "./components/NavBar.vue";
import "bootstrap/dist/css/bootstrap.min.css";
import "bootstrap/dist/js/bootstrap.js";

export default {
  components: {
    NavBar,
  },
};
</script>

<style>
body {
  background-image: url("../src/assets/images/屏幕截图 2024-05-29 123330.png");
  background-size: cover 100;
}
</style>
