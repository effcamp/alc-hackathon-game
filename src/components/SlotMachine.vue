<template>
  <div class="slot-machine">
    <div class="slot-machine-top">
      <Multiplier :multiplier="hitIndex" />
      <Pot
        class="pot"
        :pot="pot"
      />
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

    <button
      class="lever-btn btn"
      :disabled="leverPulled"
      @click="pullLever"
    ></button>
    <Points
      class="points"
      :points="pointsTotal"
    />

    <button
      class="hit-btn btn"
      @click="hitNewCard"
      :disabled="hitIndex ===5 || pointsTotal ==0 || !canHit"
    ></button>
    <button
      class="roll-btn btn"
      @click="rollOver"
      :disabled="!canRollOver"
    ></button>

    <p>{{bust ? "BUST" : ""}}</p>
    <p>{{blackJack ? "BLACKJACK" : ""}}</p>
    <!-- deck {{deck}} <br>
      virtual{{virtualDeck}} -->
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
    hitIndex: 0,
    pot: 0,
    pointsTotal: 0,
    pointsArray: [],
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
      if (this.blackJack) {
        this.pot = 0;
      }
      if (!this.leverPulled) {
        this.resetSlots();
        // this.spinReels(0);
        // this.spinReels(1);
        this.cardsToSend = this.deck.splice(0, 5);
        this.leverPulled = true;
        this.spinAllReels();
        // setTimeout(() => {
        // this.selectReels(0);
        // this.selectReels(1);
        // this.spinReels(0);
        // this.spinReels(1);
        // this.updateTotal();
        // }, 0.5);
        setTimeout(() => {
          // this.selectReels(this.hitIndex);
          // this.spinReels(this.hitIndex);
          // this.spinning.splice(this.hitIndex, 1, false);
          // this.hitIndex++;
          // this.updateTotal();
          this.hitNewCard();
        }, 5);
        setTimeout(() => {
          // this.selectReels(this.hitIndex);
          // this.spinReels(this.hitIndex);
          // this.spinning.splice(this.hitIndex, 1, false);
          // this.hitIndex++;
          // this.updateTotal();
          this.hitNewCard();
        }, 5);
      }
    },
    selectReels(i) {
      this.selectedReels[i] = true;
      this.spinning[i] = true;

      this.pointsArray.push(this.cardsToSend[i].value);
    },
    async spinReels(i) {
      this.spinning.splice(i, 1, true);

      // this.spinning[i] = true;
      await setTimeout(() => {
        this.spinning.splice(i, 1, false);
        // this.spinning[i] = false;
      }, 5000);
    },
    spinAllReels() {
      this.spinning = [true, true, true, true, true];
      this.selectedReels = [true, true, true, true, true];
    },
    hitNewCard() {
      // this.spinReels(this.hitIndex);
      this.selectReels(this.hitIndex);
      this.spinReels(this.hitIndex);
      this.spinning.splice(this.hitIndex, 1, false);
      this.hitIndex++;
      this.updateTotal();
    },
    rollOver() {
      this.leverPulled = false;
      this.canRollOver = false;
      this.canHit = false;
      switch (this.pointsTotal) {
        case 17:
          this.pot += 1 * this.hitIndex;
          break;
        case 18:
          this.pot += 1.5 * this.hitIndex;
          break;
        case 19:
          this.pot += 2 * this.hitIndex;
          break;
        case 20:
          this.pot += 2.5 * this.hitIndex;
          break;
      }
    },
    async updateTotal() {
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
      this.hitIndex = 0;
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
        this.pot = 0;
      } else if (
        this.pointsTotal === 21 ||
        (this.pointsTotal < 21 && this.hitIndex === 5)
      ) {
        // BLACKJACK
        this.pot += 3 * this.hitIndex;
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
  height: 800px;
  width: 1080px;
  display: flex;
  align-items: center;
  flex-direction: column;
  background: url(../assets/Lotto_Transparent-Foreground.png);
  background-size: 100%;
  position: relative;
}
.reels {
  width: 978px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
  /* padding: 20px; */
  position: absolute;
  top: 240px;
  left: 53px;
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
.btn:hover {
  cursor: pointer;
}
.lever-btn {
  border: none;
  background: url(../assets/BigRedButton.png);
  background-size: 100%;
  position: absolute;
  bottom: 20px;
  left: 100px;
  height: 135px;
  width: 235px;
}
.lever-btn:active {
  box-shadow: 0 5px #666;
  transform: translateY(4px);
}
.lever-btn:disabled {
  filter: grayscale(100%);
}
.hit-btn {
  position: absolute;
  border: none;
  background: url(../assets/Hit_Button.png);
  background-size: 100%;
  bottom: 20px;
  right: 200px;
  width: 150px;
  height: 100px;
}
.hit-btn:active {
  box-shadow: 0 5px #666;
  transform: translateY(4px);
}
.hit-btn:disabled {
  filter: grayscale(100%);
}
.roll-btn {
  position: absolute;
  border: none;
  background: url(../assets/Roll_Over.png);
  background-size: 100%;
  bottom: 20px;
  right: 40px;
  height: 100px;
  width: 150px;
}
.roll-btn:active {
  box-shadow: 0 5px #666;
  transform: translateY(4px);
}
.roll-btn:disabled {
  filter: grayscale(100%);
}
.points {
  position: absolute;
  bottom: 120px;
  left: 515px;
  text-align: center;
}
.pot {
  position: absolute;
  right: 45px;
  text-align: right;
  top: 65px;
}
.multiplier {
  position: absolute;
  top: 65px;
  right: 370px;
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
