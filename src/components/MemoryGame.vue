<template>
  <div class="memory-cards-container">
    <div class="card" v-for="card in gameCards" :key="card.id">
      <FlipCard
        :id="card.id"
        :bgImgBack="card.bgImgBack"
        :bgImgFront="card.bgImgFront"
        @selected="cardSelected"
      >
        <template v-slot:back><div v-html="card.back"></div></template>
        <template v-slot:front><div v-html="card.front"></div></template>
      </FlipCard>
    </div>
  </div>
</template>

<script>
import FlipCard from "./FlipCard.vue";
export default {
  props: {
    cards: {
      type: Object,
      required: true,
      validator(value) {
        return value.length > 1;
      },
    },
    revealTime: {
      type: Number,
      required: false,
      default: 1000,
      validator(value) {
        return value > 100;
      },
    },
  },
  components: {
    FlipCard,
  },
  data() {
    return {
      gameCards: [],
      firstSelection: null,
      secondSelection: null,
      pairFound: 0,
      gameOver: false,
    };
  },
  mounted() {
    this.initGame();
  },
  methods: {
    initGame() {
      this.gameCards = [];
      this.$nextTick(() => {
        this.pairFound = 0;
        this.gameOver = false;
        this.cards.forEach((card) => {
          card.flipped = false;
          card.disabled = false;
          this.gameCards.push(card);
          this.gameCards.push(card);
        });
        this.shuffleArray(this.gameCards);
      });
    },
    shuffleArray(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
    },
    resetSelection() {
      this.firstSelection = null;
      this.secondSelection = null;
    },
    cardSelected(cardRef) {
      if (this.firstSelection == null) {
        // First card selected
        this.firstSelection = cardRef;
      } else {
        // Second card selected
        this.secondSelection = cardRef;
        this.checkPair(this.firstSelection, this.secondSelection);
      }
    },
    checkPair(firstSelection, secondSelection) {
      if (firstSelection.id !== secondSelection.id) {
        // Wrong pair
        setTimeout(() => {
          firstSelection.unflip();
          firstSelection.enable();
          secondSelection.unflip();
          secondSelection.enable();
        }, this.revealTime);
      } else {
        // Pair found
        this.pairFound++;
        if (this.pairFound === this.cards.length) {
          // Stop the game
          this.gameOver = true;
          this.$emit("game-over");
        }
      }
      this.resetSelection();
    },
  },
};
</script>

<style scoped>
.memory-cards-container {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-around;
  align-items: center;
}
</style>