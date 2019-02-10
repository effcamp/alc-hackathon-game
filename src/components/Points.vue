<template>
  <div class="points">
    {{points}}
  </div>
</template>

<script>
export default {
  props: ["points"]
};
</script>

<style scoped>
.points {
  font-size: 90px;
  color: lightblue;
}
</style>
