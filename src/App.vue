<template>
  <div>
    <button @click="settings.visible = true">Settings</button>
    <memory-game
      v-if="cardsData"
      ref="memoryGameRef"
      :cards="cardsData"
      @gameOver="gameOver = true"
    />
    <!-- GAME OVER -->
    <div class="overlay" v-if="gameOver">
      Congratulations !<br /><br />
      <button class="restart" @click="restartGame">Restart</button>
    </div>
    <!-- SETTINGS -->
    <div class="overlay settings" v-if="settings.visible">
      <label for="nbCards">number of cards to find:</label>
      <input id="nbCards" type="text" v-model="settings.nbCardsToFind" />
      <button @click="restartGame">Restart</button>
      <button @click="hideSettings">Close</button>
    </div>
  </div>
</template>

<script>
import MemoryGame from "./components/MemoryGame.vue";
export default {
  name: "App",
  components: {
    MemoryGame,
  },
  data() {
    return {
      settings: {
        nbCardsToFind: 3,
        visible: false,
      },
      gameOver: false,
      cardsData: [],
    };
  },
  mounted() {
    this.$nextTick(() => {
      this.restartGame();
    });
  },
  methods: {
    generateCards(number) {
      let cards = [];
      for (let i = 0; i < number; i++) {
        cards.push({
          id: i + 1,
          front: `<img src="https://placekitten.com/${200 + i}/${
            200 + i
          }" alt="cat number ${i}" />`,
          back: "",
          /* background by SVGBackgrounds.com */
          bgImgBack: "./geometric-intersection.svg",
        });
      }
      this.cardsData = cards;
    },
    hideSettings() {
      this.settings.visible = false;
    },
    restartGame() {
      this.cardsData = [];
      this.generateCards(this.settings.nbCardsToFind);
      this.hideSettings();
      this.gameOver = false;
      this.$refs["memoryGameRef"].initGame();
    },
  },
};
</script>

<style>
#app {
  font-family: Arial;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  font-size: 26px;
}
.overlay {
  position: fixed;
  overflow: hidden;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  z-index: 2;
  display: flex;
  flex-direction: column;
  justify-content: center;
  background: rgba(0, 0, 0, 0.5);
  color: white;
}
input {
  text-align: center;
}
.settings button,
.settings input {
  font-size: 20px;
  width: 200px;
  height: 50px;
  margin: 5px auto;
}
button.restart {
  width: 200px;
  height: 50px;
  margin-left: auto;
  margin-right: auto;
}
</style>
