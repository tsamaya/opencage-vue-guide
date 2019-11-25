<template>
  <header class="app-header">
    <img src="../assets/opencage-white.svg" class="app-logo" alt="logo" />
    <p>
      OpenCage
      <b>Geocoder</b> API.
    </p>
  </header>
</template>

<style scoped>
.app-logo {
  animation: app-logo-spin 10s linear;
  height: 40vmin;
}
.app-header {
  text-align: center;
  background-color: #20b282;
  min-height: 20vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: calc(10px + 2vmin);
  color: white;
}

@keyframes app-logo-spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>
