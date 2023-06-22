<template>
  <div>
    <h1>Ring Calibration</h1>
    <div class="card-calibration">
      <div
        class="card-calibration__card"
        :style="{ height: `${creditCardHeight}px` }"
      >
        <span class="card-calibration__card-stripe"></span>
        <span class="card-calibration__card-name-long"></span>
        <span class="card-calibration__card-name-short"></span>
        Plaats kaart hier
      </div>
      <div class="card-calibration__slider">
        <input
          type="range"
          :min="sliderMin"
          :max="sliderMax"
          step="0.1"
          v-model="screenDiagonal"
          @input="$emit('pixelsPerMm', pixelsPerMm)"
        />
      </div>
    </div>

    <button @click="$emit('setPage', 'sizer')">Volgende</button>

    <p>
      {{ screenDiagonal }}"<br />
      {{ screenWidth }} screenWidth<br />
      {{ sliderMin }} sliderMin<br />
      {{ sliderMax }} sliderMax
    </p>
  </div>
</template>

<script lang="ts" setup>
import { ref, computed } from "vue";

const screenWidth = ref(screen.width);
const screenHeight = ref(screen.height);

const linearInterpolation = (
  y1: number,
  y2: number,
  x1: number,
  x2: number,
  x: number
) => {
  // y = mx + b
  const m = (y2 - y1) / (x2 - x1);
  const b = y1 - m * x1;
  return m * x + b;
};

const sliderMin = computed(() => {
  // For 390: ~6
  // For 1512: ~11
  // For 1920: ~15
  return linearInterpolation(5, 15, 390, 1920, screenWidth.value);
});

const sliderMax = computed(() => {
  // For 390: ~15
  // For 1512: ~20
  // For 1920: ~35
  return linearInterpolation(15, 35, 390, 1920, screenWidth.value);
});

const screenDiagonal = ref(20.0);
const ppi = computed(() => {
  const diagonalPixels = Math.sqrt(
    Math.pow(screenWidth.value, 2) + Math.pow(screenHeight.value, 2)
  );
  return diagonalPixels / screenDiagonal.value;
});
const pixelsPerMm = computed(() => {
  const mmPerInch = 25.4;
  return ppi.value / mmPerInch;
});

window.addEventListener("resize", () => {
  screenWidth.value = screen.width;
  screenHeight.value = screen.height;
});

const creditCardHeight = computed(() => {
  const height = 53.98; // in mm
  return height * pixelsPerMm.value;
});
</script>

<style scoped>
.card-calibration {
  position: relative;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 40px -12px;
  padding: 40px 0;
  min-height: 400px;
  width: calc(100% + 24px);
  background-color: #e9b9c4;
}
.card-calibration__card {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #fff;
  aspect-ratio: 1.58;
  border-radius: 10px;
  transition: all 0.15s;
}

.card-calibration__card:before,
.card-calibration__card:after {
  content: url('data:image/svg+xml, <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" id="Layer_1" x="0px" y="0px" viewBox="0 0 260 260" style="enable-background:new 0 0 260 260;" xml:space="preserve"><polygon points="2,104.4 130,258 258,104.4 181.4,104.4 181.4,2 78.8,2 78.8,104.4 "/></svg>');
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  width: 40px;
  font-size: 54px;
}

.card-calibration__card:before {
  top: -60px;
}

.card-calibration__card:after {
  transform: translateX(-50%) rotate(180deg);
  bottom: -60px;
}

.card-calibration__card-stripe,
.card-calibration__card-name-long,
.card-calibration__card-name-short {
  position: absolute;
  top: 10%;
  left: 0;
  height: 20%;
  width: 100%;
  background: #eee;
}

.card-calibration__card-name-long {
  top: 70%;
  left: 5%;
  height: 5%;
  width: 50%;
}

.card-calibration__card-name-short {
  top: 80%;
  left: 5%;
  height: 5%;
  width: 40%;
}

.card-calibration__slider {
  position: absolute;
  left: 0;
  bottom: 0;
  display: flex;
  height: 20px;
  width: 100%;
  background: #fff;
}

button {
  margin-top: auto;
}
</style>
