<template>
  <div class="pot">{{parseFloat(pot).toFixed(2)}}</div>
</template>

<script>
export default {
  props: ["pot"]
};
</script>

<style scoped>
.pot {
  color: rgb(97, 225, 214);
  font-size: 60px;
  /* 97 225 214 */
}
</style>
