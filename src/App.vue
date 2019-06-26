<template>
  <div id="app">
     <img alt="Vue logo" src="./assets/logo.png">
    <sheetRequest/>
  </div>
</template>

<script>
/* eslint-disable */
import sheetRequest from './components/sheetRequest.vue'

export default {
  name: 'app',
  components: {
    sheetRequest
  }
}
</script>

<style>
#app {
    display: flex;
    flex-direction: column;
    align-items: center;
}


</style>
