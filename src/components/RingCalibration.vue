<template>
  <div>
    <h1>Ring Calibration</h1>
    <div class="card-calibration">
      <div
        class="card-calibration__card"
        :style="{ height: `${creditCardHeight}px` }"
      >
        Plaats kaart hier
      </div>
      <div class="card-calibration__slider">
        <input type="range" :min="4" :max="40" step="0.1" v-model="screenDiagonal" @input="$emit('pixelsPerMm', pixelsPerMm)" />
      </div>
    </div>

    <button @click="$emit('isCalibrated', true)">Next</button>

    <!-- <p>
      {{ pixelsPerMm.toFixed(0) }} px p/mm<br />
      {{ screenDiagonal }}"
    </p> -->
  </div>
</template>

<script lang="ts" setup>
import { ref, computed } from "vue";

const screenWidth = ref(screen.width);
const screenHeight = ref(screen.height);
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
  min-height: 500px;
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
}

.card-calibration__card:before,
.card-calibration__card:after{
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  font-size: 54px;
}

.card-calibration__card:before{
  content: '⬇';
  top: -64px;   
}

.card-calibration__card:after{
  content: '⬆';
  bottom: -64px;
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
  margin-top: 32px;
}
</style>
