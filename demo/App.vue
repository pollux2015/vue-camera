<template>
  <div id="app">
    <h1>vue-camera</h1>
    <Camera/>
    <h3>How to use it</h3>
    <div class="pre-outer">
      <pre>
    import camera from 'vue-camera'
    export default {
      name: 'app',
      components: {
        Camera
      }
    }
    </pre>
    </div>
  </div>
</template>
<script>
import Camera from '@/vue-camera'

export default {
  name: 'app',
  components: {
    Camera
  }
}

</script>
<style>
body {
  margin: 0;
  padding: 0;
  background-color: #1E8AE8;
  font-size: 62.5%;
  text-align: left;
  color: #222;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width: 802px;
  padding: 0 20px 20px;
  margin: 0 auto;
  background-color: #fff;
}

h1 {
  padding-top: 20px;
  font-size: 2.4rem;
  color: #1E8AE8;
}

h3 {
  text-align: left;
  font-size: 1.8rem;
  margin: 1rem;
}

.pre-outer {
  text-align: left;
  background: #000;
  border-radius: 4px;
  padding: 5px;
  color: #F5575C;
  font-size: 1.6rem;
}

</style>
