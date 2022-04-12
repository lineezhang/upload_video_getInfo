<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <CheckH264 msg="Welcome to Your Vue.js App"/>
    <!-- <OssUpload msg="Welcome to OssUpload App"></OssUpload> -->
  </div>
</template>

<script>
import CheckH264 from './components/CheckH264.vue'
// import OssUpload from './components/OssUpload.vue'

export default {
  name: 'app',
  components: {
    CheckH264,
    OssUpload
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
