<template>
  <header><h1>My Photo Gallery</h1></header>
  <hr>
  <CameraDisplay />
</template>

<script>
import CameraDisplay from "./components/CameraDisplay.vue";

export default {
  name: 'app',
  components: {
    CameraDisplay
  }
}
</script>

<style>
header h1{
  font-size: 2em;
  text-align: center;
  color: aquamarine;
}
</style>