<template>
  <Start v-if="page === 'start'" @setPage="(event) => (page = event)" />
  <SelectPhone
    v-if="page === 'phone'"
    @setPage="(event) => (page = event)"
    @pixelsPerMm="(event) => (pixelsPerMm = event)"
  />
  <RingCalibration
    v-if="page === 'card'"
    @pixelsPerMm="(event) => (pixelsPerMm = event)"
    @setPage="(event) => (page = event)"
  />
  <RingSizer v-if="page === 'sizer'" :pixelsPerMm="pixelsPerMm" />
  <div
    style="
      position: fixed;
      right: 0;
      bottom: 0;
      font-size: 11px;
      padding: 2px 4px;
    "
  >
    {{ pixelsPerMm.toFixed(2) }}px p/mm
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import RingCalibration from "./components/RingCalibration.vue";
import RingSizer from "./components/RingSizer.vue";
import Start from "./components/Start.vue";
import SelectPhone from "./components/SelectPhone.vue";

const pixelsPerMm = ref(3);
const page = ref("start");

// If mobile, disable double tap zoom
if ("ontouchstart" in document.documentElement) {
  document.ondblclick = (e) => {
    e.preventDefault();
  };
}
</script>
