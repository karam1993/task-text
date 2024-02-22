<template>
  <img
    class="vuetify-logo"
    alt="Vuetify Logo"
      width="150"
    src="https://www.tipiteq.com/www/images/logo.png"
  >
</template>

<style>
.vuetify-logo {
  transform: rotateY(560deg);
  animation: turn 3.5s ease-out forwards 1s;
}

@keyframes turn {
  100% {
    transform: rotateY(0deg);
  }
}
</style>
