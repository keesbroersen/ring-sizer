<template>
  <div>
    <h1>Kies uw telefoon</h1>
    <select v-model="ppi" @change="onChange">
      <option :value="0" disabled selected hidden>Kies je telefoon</option>
      <option v-for="phone in phones" :key="phone.title" :value="phone.ppi">{{ phone.title }}</option>
    </select>

    <button @click="$emit('setPage', 'sizer')">Volgende</button>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue';
import devices from './devices.json';

interface phone {
  title: string;
  ppi: number;
}

const emit = defineEmits(['pixelsPerMm','setPage']);
const ppi = ref(0);
const phones = devices.phones as phone[];

const onChange = () => {
  const pixelsPerMm = ppi.value / 25.4;
  emit('pixelsPerMm', pixelsPerMm);
}
</script>

<style scoped>
.start {
  display: flex;
  align-items: center;
  gap: 16px;
}

select {
  margin: 32px 0 16px;
  min-height: 50px;
  -webkit-appearance: none;
  background: #eee;
  width: 100%;
  border: none;
  border-radius: 5px;
  padding: 8px 16px;
  font-size: 1em;
}
</style>
