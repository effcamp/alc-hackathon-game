<template>
  <div class="slot-machine">
    <div class="slot-machine-top">
      <div class="title">Slot Machine</div>
      <Multiplier :multiplier="multiplier" />
      <Pot :pot="pot" />
    </div>
    <div class="reels">
      <Reel
        v-for="i in 5"
        :card="cardsToSend[i-1]"
        :key="i-1"
        :showCard="selectedReels[i-1]"
        :spinning="spinning[i-1]"
        :virtualDeck="virtualDeck"
      />
    </div>
    <div class="controls">

      <button
        class="lever-btn btn"
        :disabled="leverPulled"
        @click="pullLever"
      >Play</button>
      {{pointsTotal || 0}}
      <Points :points="pointsTotal" />

      <button
        class="hit-btn btn"
        @click="hitNewCard"
        :disabled="hitIndex ===5 || pointsTotal === 0 || !canHit"
      >Hit</button>
      <button
        class="roll-btn btn"
        @click="rollOver"
        :disabled="!canRollOver"
      >Roll Over</button>

      <p>{{bust ? "BUST" : ""}}</p>
      <p>{{blackJack ? "BLACKJACK" : ""}}</p>
      <!-- deck {{deck}} <br>
      virtual{{virtualDeck}} -->
    </div>
  </div>
</template>

<script>
import Reel from "./Reel.vue";
import Lever from "./Lever.vue";
import Points from "./Points.vue";
import Pot from "./Pot.vue";
import Multiplier from "./Multiplier.vue";

export default {
  components: {
    Reel,
    Lever,
    Points,
    Pot,
    Multiplier
  },
  data: () => ({
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
    virtualDeck: undefined,
    cardsToSend: [],
    spinning: [false, false, false, false, false],
    hitIndex: 2,
    pot: 0,
    pointsTotal: 0,
    pointsArray: [],
    multiplier: 1,
    selectedReels: [false, false, false, false, false],
    leverPulled: false,
    canRollOver: false,
    canHit: true,
    bust: false,
    blackJack: false,
    multiplier: 1,
    pot: 0
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
        this.resetSlots();
        // this.spinReels(0);
        // this.spinReels(1);
        this.cardsToSend = this.deck.splice(0, 5);
        this.leverPulled = true;
        setTimeout(() => {});
        this.selectReels(0);
        this.selectReels(1);
        this.updateTotal();
      }
    },
    selectReels(i) {
      this.selectedReels[i] = true;
      this.spinning[i] = true;

      this.pointsArray.push(this.cardsToSend[i].value);
    },
    hitNewCard() {
      // this.spinReels(this.hitIndex);
      this.selectReels(this.hitIndex);
      this.hitIndex++;
      this.updateTotal();
    },
    rollOver() {
      this.leverPulled = false;
      this.canRollOver = false;
      this.canHit = false;
      switch (this.pointsTotal) {
        case 17:
          this.pot += 1;
          break;
        case 18:
          this.pot += 2;
          break;
        case 19:
          this.pot += 3;
          break;
        case 20:
          this.pot += 4;
          break;
      }
    },
    updateTotal() {
      setTimeout(() => {
        if (this.pointsArray.length !== 0) {
          this.pointsTotal = this.pointsArray.reduce((prev, cur, i) =>
            this.selectedReels[i] ? prev + cur : prev
          );
        }
      }, 5000);
    },
    resetSlots() {
      this.blackJack = false;
      this.canRollOver = false;
      this.bust = false;
      this.hitIndex = 2;
      this.pointsTotal = 0;
      this.createDeck();
      this.shuffle(this.deck);
      this.virtualDeck = this.deck.slice(0);
      this.pointsArray = [];
      this.selectedReels = [false, false, false, false, false];
      this.spinning = [false, false, false, false, false];

      this.canHit = true;
    }
    // spinReels(card) {
    //   this.spinning[card] = true;

    //   setTimeout(() => {
    //     this.spinning[card] = false;
    //   }, 5000);
    // }
  },
  computed: {},
  watch: {
    pointsTotal() {
      if (this.pointsTotal > 16 && this.pointsTotal < 21) {
        this.canRollOver = true;
      } else if (this.pointsTotal > 21) {
        //BUST
        this.bust = true;
        this.canHit = false;
        this.canRollOver = false;
        this.leverPulled = false;
      } else if (this.pointsTotal === 21) {
        // BLACKJACK
        this.pot += 5;
        this.blackJack = true;
        this.canHit = false;
        this.leverPulled = false;
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
.slot-machine {
  height: 600px;
  display: flex;
  align-items: center;
  flex-direction: column;
}
.reels {
  width: 1080px;
  margin: 0 auto;
  border: 1px solid black;
  display: flex;
  justify-content: space-between;
  padding: 20px;
}
.slot-machine-top {
  display: flex;
  height: 75px;
}
.slot-machine-top > * {
  margin: 0 20px;
}
.controls {
  display: flex;
  justify-content: center;
  align-items: center;
}
.controls > * {
  margin: 0 10px;
}
.btn {
  height: 50px;
  width: 100px;
  /* background: url(../assets/Background-21912.jpg);
  background-size: 100%; */
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
