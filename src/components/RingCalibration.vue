<template>
  <div>
    <h1>Ring Calibration</h1>
    <div class="card-calibration">
      <div
        class="card-calibration__card"
        :style="{ height: `${creditCardHeight}px` }"
      >
        <div class="card-calibration__card-content">
          <span class="card-calibration__card-stripe"></span>
          <span class="card-calibration__card-name-long"></span>
          <span class="card-calibration__card-name-short"></span>
          <p>
            Plaats kaart hier<br/>
            <small>(het gaat om de hoogte)</small>
          </p>
        </div>
      </div>
      <div class="card-calibration__slider">
        <input
          type="range"
          :min="sliderMin"
          :max="sliderMax"
          step="0.05"
          v-model="screenSizeInch"
          @input="$emit('pixelsPerMm', pixelsPerMm)"
        />
      </div>
    </div>

    <button @click="$emit('setPage', 'sizer')">Volgende</button>

    <!-- <p>
      {{ screenSizeInch }}"<br />
      {{ screenWidth }} screenWidth<br />
      {{ sliderMin }} sliderMin<br />
      {{ sliderMax }} sliderMax<br />
      {{ dpr }} devicePixelRatio
    </p> -->
  </div>
</template>

<script lang="ts" setup>
import { ref, computed } from "vue";

const screenWidth = ref(screen.width);
const screenHeight = ref(screen.height);

const sliderMin = computed(() => {
  if(screenWidth.value < 768) return (screenSizeInch.value = 10) && 4;
  if(screenWidth.value < 1920) return (screenSizeInch.value = 10) && 6;
  return (screenSizeInch.value = 20) && 11;
});

const sliderMax = computed(() => {
  if(screenWidth.value < 768) return 7;
  if(screenWidth.value < 1920) return 20;
  return 35;
});

const screenSizeInch = ref(20);

const ppi = computed(() => {
  const diagonalPixels = Math.sqrt(
    Math.pow(screenWidth.value, 2) + Math.pow(screenHeight.value, 2)
  );
  return diagonalPixels / screenSizeInch.value;
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
  padding: 50px 0 70px;
  min-height: 400px;
  width: calc(100% + 24px);
  background-color: #e9b9c4;
}

.card-calibration p {
  line-height: 120%;
}

.card-calibration small {
  font-size: 10px;
}

.card-calibration__card {
  position: relative;
  display: flex;
  justify-content: center;
  width: 100%;
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

.card-calibration__card-content {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
  background-color: #fff;
  aspect-ratio: 1.58;
  border-radius: 10px;
  transition: all 0.15s;
}

.card-calibration__card-content:before,
.card-calibration__card-content:after {
  content: "";
  position: absolute;
  left: -50vw;
  width: 250vw;
  height: 1px;
  background: rgba(255, 255, 255, 0.3);
}

.card-calibration__card-content:before {
  top: 0;
}

.card-calibration__card-content:after {
  bottom: 0;
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

/* for mobile */
@media (max-width: 767px) {
  .card-calibration {
    min-height: 540px;
  }

  .card-calibration__card {
    justify-content: start;
  }

  .card-calibration__card-content {
    transform: translateX(-33%);
  }
}
</style>
