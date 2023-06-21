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
          step="1"
        />
      </div>
    </div>

    <table class="ring-sizer-table">
      <tr>
        <th>Maat</th>
        <th>Binnendiameter</th>
        <th>Omtrek</th>
      </tr>
      <tr
        v-for="ring in ringSizeList"
        :key="ring.size"
        :class="{ active: ring.isActive }"
      >
        <td>{{ ring.size }}</td>
        <td>{{ ring.diameter }} mm</td>
        <td>{{ ring.circumference.toFixed(0) }}</td>
      </tr>
    </table>
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
const sliderValue = ref(16); // in mm

const ringSizeInPx = computed(() => {
  return sliderValue.value * props.pixelsPerMm;
});

const ringSizeList = computed(() => {
  // const smallestSize = 44;
  // const biggestSize = 69;

  // Based on circumference in mm 
  // const ringSizeList = Array.from(
  //   { length: biggestSize - smallestSize + 1 },
  //   (_, i) => {
  //     const size = smallestSize + i;
  //     return {
  //       size: size,
  //       mm: size / Math.PI,
  //       isActive: false,
  //     };
  //   }
  // );

  // Based on diameter in mm
  const ringSizeList = Array.from(
    { length: 22 - 13 + 1 },
    (_, i) => {
      const diameter = 13 + i;
      return {
        size: diameter,
        diameter,
        circumference: diameter * Math.PI,
        isActive: false,
      };
    }
  );

  const closestRingSize = ringSizeList.reduce((prev, curr) => {
    return Math.abs(curr.diameter - sliderValue.value) <
      Math.abs(prev.diameter - sliderValue.value)
      ? curr
      : prev;
  });

  const ringIndex = ringSizeList.findIndex(
    (ring) => ring.size === closestRingSize.size
  );
  
  const list = ringSizeList.slice(ringIndex - 1, ringIndex + 2);
  ringSizeList[ringIndex].isActive = true;

  return list;
});
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
  min-height: 300px;
  width: calc(100% + 24px);
  background-color: #e9b9c4;
  /* background-image: linear-gradient(to right, #ccc 1px, transparent 1px),
    linear-gradient(to bottom, #ccc 1px, transparent 1px); */
}

.ring-sizer:before,
.ring-sizer:after {
  content: "";
  position: absolute;
  background: rgba(255, 255, 255, 0.2);
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
  box-shadow: 0 0 0 5px rgba(0,0,0,0.25);
  transition: all 0.15s;
}

.ring-sizer__circle * {
  transition: all 0.15s;
}

.ring-sizer__circle:before,
.ring-sizer__circle:after {
  content: "";
  position: absolute;
  top: -50vh;
  height: 100vh;
  width: 1px;
  background: rgba(255, 255, 255, 0.4);
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

.ring-sizer-table {
  border-collapse: collapse;
  border-top: 1px solid #eee;
  border-right: 1px solid #eee;
  width: 100%;
}

.ring-sizer-table th,
.ring-sizer-table td {
  padding: 10px 0;
  width: calc(100% / 3);
  text-align: center;
  border-left: 1px solid #eee;
  border-bottom: 1px solid #eee;
}

.ring-sizer-table .active {
  background: #eee;
  font-weight: bold;
}
</style>
