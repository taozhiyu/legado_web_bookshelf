<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: "app",
  components: {},
  beforeCreate() {
    this.$store.commit("setMiniInterface", window.innerWidth < 750);
    window.onresize = () => {
      this.$store.commit("setMiniInterface", window.innerWidth < 750);
    };
  },
};
</script>

<style lang="scss" scoped>
@import "./assets/styles/dark.scss";
@import "./assets/styles/light.scss";

#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: var(--app-text-color);
  margin: 0;
  height: 100%;
}
</style>
