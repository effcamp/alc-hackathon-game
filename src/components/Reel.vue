<template>
  <div class="reel">
    <Card :card="card" />
  </div>
</template>

<script>
import Card from "./Card.vue";
export default {
  props: ["card"],
  components: {
    Card
  }
};
</script>

<style>
.reel {
  border: 1px dotted green;
  width: 200px;
  height: 400px;
  padding-top: 50px;
}
</style>
