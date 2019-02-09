<template>
  <div>
    <div class="slot-machine">
      <div class="reels">
        <Reel
          v-for="i in 5"
          :card="cardsToSend[i-1]"
          :key="i-1"
        />
      </div>
      <Lever @click.native="pullLever" />
    </div>
    <div class="cardPointsTotal">
      {{pointsTotal}}
    </div>
    <button>Hit</button>
    <button>Roll Over</button>
    <!-- {{deck}} -->
  </div>
</template>

<script>
import Reel from "./Reel.vue";
import Lever from "./Lever.vue";

export default {
  components: {
    Reel,
    Lever
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
      { name: "Jack", value: 10, face: "jack" },
      { name: "Queen", value: 10, face: "queen" },
      { name: "King", value: 10, face: "king" }
    ],
    deck: undefined,
    cardsToSend: [],
    hitIndex: 2,
    pot: 0,
    pointsTotal: 0,
    multiplier: 1,
    selectedReels: [false, false, false, false, false],
    leverPulled: false
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
            value: n.value,
            suit,
            face: n.face
          }))
        )
        .flat(1);
    },
    pullLever() {
      if (!this.leverPulled) {
        this.cardsToSend = this.deck.splice(0, 5);
        this.leverPulled = true;
        this.selectReels(0);
        this.selectReels(1);

        this.createDeck();
        this.shuffle(this.deck);
      }
    },

    selectReels(i) {
      if (!this.selectedReels[i]) {
        this.selectedReels[i] = true;
        this.pointsTotal += this.cardsToSend[i].value;
      }
    }
  },
  computed: {},
  watch: {
    leverPulled() {
      if (this.leverPulled) {
      }
    }
  },
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
.slot-machine {
  display: flex;
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
