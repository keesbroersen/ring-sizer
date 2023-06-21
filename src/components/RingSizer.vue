<template>
  <div>
    <h1>Ring Sizer</h1>
    <div
      class="ring-sizer"
      :style="{ backgroundSize: `${pixelsPerMm}px ${pixelsPerMm}px` }"
    >
      <div
        class="ring-sizer__circle"
        :style="{
          width: ringSizeInPx + 'px',
          height: ringSizeInPx + 'px',
        }"
      >
        <div
          class="ring-sizer__marker"
          :style="{ width: ringSizeInPx + 'px' }"
        >
          <span>{{ sliderValue }} mm</span>
        </div>
      </div>
      <div class="ring-sizer__slider">
        <input
          type="range"
          :min="sliderMin"
          :max="sliderMax"
          v-model="sliderValue"
          @input="updateMarkerPosition"
          step="0.1"
        />
      </div>
    </div>
    <div class="ring-sizer-list">list</div>
  </div>
</template>

<script lang="ts" setup>
import { ref, computed } from "vue";

const props = defineProps({
  pixelsPerMm: {
    type: Number,
    required: true,
  },
});

const sliderMin = 15; // in mm
const sliderMax = 35; // in mm
const sliderValue = ref(sliderMin + sliderMax / 2); // in mm


const ringSizeInPx = computed(() => {
  return (sliderValue.value * props.pixelsPerMm);
});

function updateMarkerPosition() {
  // do nothing
}
</script>

<style scoped>
.ring-sizer {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 40px -12px;
  padding: 40px 0;
  min-height: 500px;
  width: calc(100% + 24px);
  background-color: #fff;
  background-image: linear-gradient(to right, #ccc 1px, transparent 1px),
    linear-gradient(to bottom, #ccc 1px, transparent 1px);
}

.ring-sizer:before,
.ring-sizer:after {
  content: "";
  position: absolute;
  background: #999;
}

.ring-sizer:before{
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  height: 100%;
  width: 1px;
}

.ring-sizer:after{
  top: 50%;
  left: 0;
  transform: translateY(-50%);
  height: 1px;
  width: 100%;
}

.ring-sizer__circle {
  position: relative;
  overflow: hidden;
  z-index: 2;
  border-radius: 50%;
  background: #fff;
}

/* .ring-sizer__circle:before {
  content: '';
  position: absolute;
  top: -20%;
  left: -20%;
  height: 140%;
  width: 140%;
  background: #fff;
  filter: blur(5px);
  opacity: 0.8;
} */

.ring-sizer__marker {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 1px;
  background-color: rgba(0,0,0,0.5);
}
.ring-sizer__marker span {
  padding: 0 8px;
  background: #fff;
}

.ring-sizer__slider {
  position: absolute;
  left: 0;
  bottom: 0;
  display: flex;
  height: 4px;
  width: 100%;
}
</style>
