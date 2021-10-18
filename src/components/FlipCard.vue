<template>
  <div
    class="flip-card-container"
    :class="{ 'card-flipped': flipped }"
    :style="{ width, height }"
    @click="selectCard"
  >
    <div class="flip-front" :style="bgStyle.front">
      <slot name="front"></slot>
    </div>
    <div class="flip-back" :style="bgStyle.back">
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
      required: false,
      default: "150px",
    },
    height: {
      type: String,
      required: false,
      default: "200px",
    },
    bgImgBack: {
      type: String,
      required: false,
    },
    bgImgFront: {
      type: String,
      required: false,
    },
  },
  data() {
    return {
      flipped: false,
      disabled: false,
    };
  },
  computed: {
    bgStyle: function () {
      const bgStyle = { back: {}, front: {} };
      if (this.bgImgBack) {
        bgStyle.back.backgroundImage = `url(${this.bgImgBack})`;
      }
      if (this.bgImgFront) {
        bgStyle.front.backgroundImage = `url(${this.bgImgFront})`;
      }
      return bgStyle;
    },
  },
  methods: {
    selectCard() {
      if (!this.disabled) {
        this.flipped = true;
        this.disabled = true;
        this.$emit("selected", this);
      }
    },
    unflip() {
      this.flipped = false;
    },
    disable() {
      this.disabled = true;
    },
    enable() {
      this.disabled = false;
    },
  },
};
</script>

<style scoped>
.flip-card-container {
  position: relative;
  perspective: 1000px;
  cursor: pointer;
  display: inline-block;
  margin: 15px;
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