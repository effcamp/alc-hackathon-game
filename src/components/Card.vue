<template>
  <div
    class="card"
    v-if="card && showCard"
  >
    <img
      class="number"
      :src="number"
      :alt="card.number"
    >
    <img
      class="suit"
      :src="suit"
      :alt="card.suit"
    >
    <!-- 
    {{card.name}}
    <br>
    {{card.value}}
    {{card.face}} -->

  </div>
</template>

<script>
export default {
  props: ["card", "showCard"],
  data: () => ({
    suit: ``,
    number: ``,
    showTheCard: false
  }),
  watch: {
    card() {
      this.suit = require(`../assets/cards/suits/${this.card.suit}.png`);
      if (this.card.suit === "clubs" || this.card.suit === "spades") {
        if (!this.card.face) {
          this.number = require(`../assets/cards/numbers/black/card_${
            this.card.value
          }.png`);
        } else {
          this.number = require(`../assets/cards/numbers/black/card_${
            this.card.face
          }.png`);
        }
      } else {
        if (!this.card.face) {
          this.number = require(`../assets/cards/numbers/red/card_${
            this.card.value
          }.png`);
        } else {
          this.number = require(`../assets/cards/numbers/red/card_${
            this.card.face
          }.png`);
        }
      }
    }
    // showCard() {
    //   if (this.showCard) {
    //     this.showTheCard = true;
    //   }
    // }
  }
};
</script>

<style>
.card {
  background: url(../assets/cards/cardTemplate.png);
  background-size: 100%;
  background-repeat: no-repeat;
  height: 300px;
  z-index: -3;
}
.suit {
  width: 50px;
  z-index: 5;
}
</style>
