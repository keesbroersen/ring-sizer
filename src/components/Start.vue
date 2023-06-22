<template>
  <div>
    <h1>Welkom</h1>
    <p>Wilt u uw ringmaat meten? Gebruik onze handige methode met een creditcard of kies uw telefoon uit de lijst. Het is eenvoudig, nauwkeurig en bespaart u tijd. Bestel nu uw perfect passende ring!</p>
    <div class="start">
      <button @click="$emit('setPage', 'card')">Gebruik een pasje</button>
      <button @click="$emit('setPage', 'phone')">Kies je telefoon</button>
    </div>
  </div>
</template>

<style scoped>
.start {
  display: flex;
  align-items: center;
  gap: 16px;
}

p {
  margin: 32px 0;
}

button {
  min-height: 100px;
}
</style>
