<template>
  <div id="app" class="body">
    <App></App>
  </div>
</template>

<script>
import Vue from "vue";
import App from "../App.vue";

export default Vue.extend({
  name: "Home",
  components: {
    App
  },
  created() {
    // Font Awesome
    document.write(
      '<link rel="stylesheet" href="https://kit-pro.fontawesome.com/releases/v5.11.2/css/pro.min.css">'
    );
  }
});
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  list-style: none;
  outline: none;
  user-select: none;
  color: inherit;
  text-decoration: none;
}

body {
  background: #f2f2f2;
  color: #868686;

  .body {
    padding: 5vh 5vw;
  }
}
</style>
