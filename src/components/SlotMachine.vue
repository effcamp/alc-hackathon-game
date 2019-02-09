<template>
  <div class="slot-machine">
    <Reel />
    <Reel />
    <Reel />
    <Reel />
    <Reel />
  </div>
</template>

<script>
import Reel from "./Reel.vue";

export default {
  components: {
    Reel
  },
  data: () => ({
    msg: "Hello!",
    suits: ["clubs", "hearts", "spades", "diamonds"],
    numbers: [
      "1",
      "2",
      "3",
      "4",
      "5",
      "6",
      "7",
      "8",
      "9",
      "10",
      "11",
      "12",
      "13"
    ],
    deck: []
  }),
  methods: {},
  computed: {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.slot-machine {
  width: 1080px;
  margin: 0 auto;
  border: 1px solid black;
  display: flex;
  justify-content: space-between;
  padding: 20px;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
