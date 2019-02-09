<template>
  <div class="slot-machine">
    <button @click="sendFirstCard">test</button>
    <!-- {{deck}} -->
    <p
      v-for="(reel, i) in selectedReels"
      :key="i"
    >{{reel}}</p>
    <div class="reels">
      <Reel
        v-for="i in 5"
        @click.native="selectReels(i-1)"
        :card="cardsToSend[i-1]"
        :key="i"
      />

    </div>
    {{points}}

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
      { name: "Ace", value: 1 },
      { name: "2", value: 2 },
      { name: "3", value: 3 },
      { name: "4", value: 4 },
      { name: "5", value: 5 },
      { name: "6", value: 6 },
      { name: "7", value: 7 },
      { name: "8", value: 8 },
      { name: "9", value: 9 },
      { name: "10", value: 10 },
      { name: "Jack", value: 10 },
      { name: "Queen", value: 10 },
      { name: "King", value: 10 }
    ],
    deck: undefined,
    // cardsToSend: [],
    cardsToSend: [1, 2, 3, 4, 5],
    pot: 0,
    points: 0,
    multiplier: 1,
    selectedReels: [false, false, false, false, false]
  }),
  methods: {
    shuffle(a) {
      for (let i = a.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [a[i], a[j]] = [a[j], a[i]];
      }
      return a;
    },
    createDeck() {
      this.deck = this.suits
        .map(suit =>
          this.numbers.map(n => ({
            name: `${n.name} of ${suit}`,
            value: n.value
          }))
        )
        .flat(1);
    },
    sendFirstCard() {
      this.cardsToSend = this.deck.splice(0, 5);
      this.createDeck();
      this.shuffle(this.deck);
    },
    selectReels(reelIndex) {
      this.selectedReels[reelIndex] = true;
      this.totalPoints();
    },
    totalPoints() {
      this.points = this.cardsToSend.reduce((prev, curr, i) => {
        // if (this.selectedReels[i]) {
        console.log(prev.value, curr);
        return prev + curr;
        // } else {/
        // return prev;
        // }
      }, 0);

      // this.selectedReels.reduce((reel, i) =>
      //   reel ? this.cardsToSend[i].value : 0
      // );
    }
  },
  computed: {},
  watch: {},
  mounted() {
    this.createDeck();
    this.shuffle(this.deck);
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.reels {
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
