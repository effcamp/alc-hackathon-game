<template>
  <div id="app">
    <Points />
    <SlotMachine />

  </div>
</template>

<script>
import Points from "./components/Points.vue";
import SlotMachine from "./components/SlotMachine.vue";

export default {
  name: "app",
  components: {
    SlotMachine,
    Points
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
