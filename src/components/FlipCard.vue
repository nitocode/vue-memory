<template>
  <div
    class="flip-card-container"
    :class="{ 'card-flipped': flipped }"
    :style="{ width, height }"
    @click="selectCard"
  >
    <div class="flip-front">
      <slot name="front"></slot>
    </div>
    <div class="flip-back">
      <slot name="back"></slot>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    id: {
      type: Number,
      required: true,
    },
    width: {
      type: String,
      required: true,
    },
    height: {
      type: String,
      required: true,
    },
  },
  data() {
    return {
      flipped: false,
    };
  },
  methods: {
    selectCard() {
      this.flipped = true;
      this.$emit("selected", this.id);
    },
    unflip() {
      this.flipped = false;
    },
  },
};
</script>

<style>
.flip-card-container {
  position: relative;
  perspective: 1000px;
  cursor: pointer;
}
.flip-front,
.flip-back {
  position: absolute;
  overflow: hidden;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  transition: transform 1s;
  backface-visibility: hidden;
}
.flip-front {
  transform: rotateY(180deg);
}
.card-flipped .flip-back {
  transform: rotateY(180deg);
}
.card-flipped .flip-front {
  transform: rotateY(360deg);
}
</style>