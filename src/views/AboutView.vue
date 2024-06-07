<template>
  <div class="about">
    <div class="page-wrapper">
      <div class="lead">
        <div class="lead-text">Movie Vault</div>
        <p class="sublead-text">Download your favourite movies with just one click</p>
        <br>
        <p class="sublead-text">NOTE: This is just an educational project to learn Vue.js and this does not support piracy nor provides any movie download links.</p>
        <p class="sublead-text">We do not support piracy. Always purchase movies.</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
@media (min-width: 1024px) {
  .about {
    min-height: 100vh;
    display: flex;
    align-items: center;
  }
}

.page-wrapper {
  min-height: 80vh;
  margin-top: 5rem;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
}

div.lead {
  text-align: center;
}

.lead .lead-text {
  font-size: 3.2rem;
  color: white;
  font-weight: 700;
  line-height: 5rem;
}

.lead .sublead-text {
  font-size: 1.2rem;
  line-height: 3rem;
}
</style>
