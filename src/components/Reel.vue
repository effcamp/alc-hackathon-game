<template>
  <div class="reel">
    Reel- this is another
    <Card />
  </div>
</template>

<script>
import Card from "./Card.vue";
export default {
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
