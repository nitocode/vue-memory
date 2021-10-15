<template>
  <FlipCard />
</template>

<script>
import FlipCard from "./components/FlipCard.vue";
export default {
  name: "App",
  components: {
    FlipCard: FlipCard,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
