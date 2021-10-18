<template>
  <div>
    <memory-game
      ref="memoryGameRef"
      :cards="cardsData"
      @gameOver="gameOver = true"
    />
    <div v-if="gameOver">
      Congratulations !<br /><br />
      <button @click="restartGame">Restart</button>
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
      gameOver: false,
      /* background by SVGBackgrounds.com */
      cardsData: [
        {
          id: 1,
          front: `<img src="https://placekitten.com/150/202" alt="" />`,
          back: "",
          bgImgBack: "./geometric-intersection.svg",
        },
        {
          id: 2,
          front: `<img src="https://placekitten.com/150/220" alt="" />`,
          back: "",
          bgImgBack: "./geometric-intersection.svg",
        },
        {
          id: 3,
          front: `<img src="https://placekitten.com/150/200" alt="" />`,
          back: "",
          bgImgBack: "./geometric-intersection.svg",
        },
        {
          id: 4,
          front: `<img src="https://placekitten.com/150/210" alt="" />`,
          back: "",
          bgImgBack: "./geometric-intersection.svg",
        },
      ],
    };
  },
  methods: {
    restartGame() {
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
</style>
