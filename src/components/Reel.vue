<template>
  <div class="reel">
    <!-- {{spinning}} -->
    <Card
      :showCard="showCard"
      :card="cardToShow"
    />
  </div>
</template>

<script>
import Card from "./Card.vue";
export default {
  props: ["card", "showCard", "spinning", "virtualDeck"],
  components: {
    Card
  },
  data: () => ({
    cardToShow: undefined,
    stillSpinning: true
  }),
  watch: {
    spinning() {
      if (this.spinning === true) {
        let reelSpin = setInterval(() => {
          this.cardToShow = this.virtualDeck[Math.floor(Math.random() * 52)];
          // console.log(Math.floor(Math.random() * 52));
        }, 200);
        setTimeout(() => {
          window.clearInterval(reelSpin);
          this.cardToShow = this.card;
        }, 5000);
      }
    }
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
