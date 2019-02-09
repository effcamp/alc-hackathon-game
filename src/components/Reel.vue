<template>
  <div class="reel">
    Reel <br>
    {{card && card.name}}
    <Card />
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
}
</style>
