<template>
  <div id="app">
    <div class="slot-face">
      <SlotMachine />
    </div>

  </div>
</template>

<script>
import SlotMachine from "./components/SlotMachine.vue";

export default {
  name: "app",
  components: {
    SlotMachine
  }
};
</script>

<style>
* {
  box-sizing: border-box;
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
