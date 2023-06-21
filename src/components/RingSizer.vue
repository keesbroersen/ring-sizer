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
        <span class="ring-sizer__circle-lines"></span>
        <div class="ring-sizer__marker" :style="{ width: ringSizeInPx + 'px' }">
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
          step="0.25"
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

const sliderMin = 14; // in mm
const sliderMax = 22; // in mm
const sliderValue = ref(18); // in mm

const ringSizeInPx = computed(() => {
  return sliderValue.value * props.pixelsPerMm;
});

function updateMarkerPosition() {
  // do nothing
}
</script>

<style scoped>
.ring-sizer {
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
  /* background-image: linear-gradient(to right, #ccc 1px, transparent 1px),
    linear-gradient(to bottom, #ccc 1px, transparent 1px); */
}

.ring-sizer:before,
.ring-sizer:after {
  content: "";
  position: absolute;
  background: rgba(255, 255, 255, 0.1);
}

.ring-sizer:before {
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  height: 100%;
  width: 1px;
}

.ring-sizer:after {
  top: 50%;
  left: 0;
  transform: translateY(-50%);
  height: 1px;
  width: 100%;
}

.ring-sizer__circle {
  position: relative;
  z-index: 2;
  border-radius: 50%;
  background: #fff;
}

.ring-sizer__circle:before,
.ring-sizer__circle:after {
  content: "";
  position: absolute;
  top: -50vh;
  height: 100vh;
  width: 1px;
  background: rgba(255, 255, 255, 0.3);
}

.ring-sizer__circle:before {
  left: 0;
}

.ring-sizer__circle:after {
  right: 0;
}

.ring-sizer__circle-lines:before,
.ring-sizer__circle-lines:after {
  content: "";
  position: absolute;
  left: -50vw;
  width: 150vw;
  height: 1px;
  background: rgba(255, 255, 255, 0.3);
}

.ring-sizer__circle-lines:before {
  top: 0;
}

.ring-sizer__circle-lines:after {
  bottom: 0;
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
  background-color: rgba(0, 0, 0, 0.5);
}
.ring-sizer__marker span {
  padding: 0 2px;
  background: #fff;
  font-size: 0.6em;
}

.ring-sizer__slider {
  position: absolute;
  left: 0;
  bottom: 0;
  display: flex;
  height: 20px;
  width: 100%;
  background: #fff;
}
</style>
