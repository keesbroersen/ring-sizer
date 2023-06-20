<template>
  <div>
    <h1>Ring Sizer</h1>
    <div class="ring-sizer" :style="{ backgroundSize: `${ oneCmInPx }px ${ oneCmInPx }px`}">
      <div class="ring-circle" :style="{ width: sliderValueInPx + 'px', height: sliderValueInPx + 'px' }">
        <div class="ring-marker" :style="{ width: sliderValueInPx + 'px' }"></div>
      </div>
      <div class="ring-slider">
        <input type="range" :min="sliderMin" :max="sliderMax" v-model="sliderValueInPx" @input="updateMarkerPosition">
      </div>
      <div class="ring-size">{{ ringSizeInMm }} mm</div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref, computed } from 'vue'

const sliderMin = 30 // in pixels
const sliderMax = 200 // in pixels
const sliderValueInPx = ref(sliderMin);

const oneCmInPx = computed(() => {
  const dpi = window.devicePixelRatio * 96;
  return 10 * dpi / 25.4;
});

const ringSizeInMm = computed(() => {
  const dpi = window.devicePixelRatio * 96;
  const mmPerInch = 25.4;
  return (sliderValueInPx.value / dpi * mmPerInch).toFixed(0);
})

function updateMarkerPosition() {
  // do nothing
}
</script>

<style scoped>
.ring-sizer {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 500px;
  min-width: 500px;
  margin-top: 50px;
  background-color: #f5f5f5;
  background-image: linear-gradient(to right, #ccc 1px, transparent 1px),
                    linear-gradient(to bottom, #ccc 1px, transparent 1px);
  background-size: 20px 20px;
}

.ring-circle {
  position: relative;
  border-radius: 50%;
  background-color: #ddd;
  margin-bottom: 20px;
}

.ring-marker {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 100%;
  height: 2px;
  border-radius: 50%;
  background-color: #f00;
}

.ring-slider {
  width: 100%;
}

.ring-size {
  font-size: 24px;
  font-weight: bold;
}
</style>