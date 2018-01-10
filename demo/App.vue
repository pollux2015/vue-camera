<template>
  <div id="app">
    <Camera/>
  </div>
</template>

<script>
import Camera from '@/camera'

export default {
  name: 'app',
  components: {
    Camera
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
