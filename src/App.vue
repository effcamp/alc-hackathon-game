<template>
  <div id="app">
    <Points />
    <div class="slot-face">
      <SlotMachine />
    </div>

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
* {
  box-sizing: border-box;
}
#app {
  border: 1px solid red;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.slot-face {
  display: flex;
  justify-content: center;
}
</style>
