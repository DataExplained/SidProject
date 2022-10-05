<template>
  <main>
    <GetAllData></GetAllData>
  </main>
</template>

<script>
import GetAllData from "./components/GetAllData.vue";

export default {
  name: "App",
  components: {
    GetAllData,
  },

  data() {
    return {
      ListOfAirports: [],
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
