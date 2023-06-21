<template>
  <RingCalibration v-if="!isCalibrated" @pixelsPerMm="(event) => pixelsPerMm = event" @isCalibrated="(event) => isCalibrated = event" />
  <RingSizer v-else :pixelsPerMm="pixelsPerMm" />
</template>

<script setup lang="ts">
import { ref } from 'vue';
import RingCalibration from './components/RingCalibration.vue';
import RingSizer from './components/RingSizer.vue'

const pixelsPerMm = ref(3);
const isCalibrated = ref(false);
</script>

