<template>
  <live-stock-exchange/>
</template>

<script>
import LiveStockExchange from "./components/LiveStockExchange.vue";

export default {
  name: "App",
  components: {
    LiveStockExchange,
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
