<template>
  <div id="app">
    <div class="page-container">
    <img alt="Lanchnation logo" src="./assets/img/logo.png" class="logo">
      <section class="avatar-container">
        <figure>
          <img alt="Shep" src="./assets/svg/trophy.svg" class="avatar"/>
          <figcaption>Shep</figcaption>
          <!-- <object type="image/svg+xml" data="./assets/svg/trophy.svg"></object> -->
        </figure>
        <figure>
          <img alt="Hicks" src="./assets/img/hicks2.jpg" class="avatar"/>
          <figcaption>Hicks</figcaption>
        </figure>
         <figure>
          <img alt="Jack" src="./assets/img/jack.jpg" class="avatar"/>
          <figcaption>Jack</figcaption>
        </figure>
         <figure>
          <img alt="Cragg" src="./assets/img/cragg.jpg" class="avatar"/>
          <figcaption>Cragg</figcaption>
        </figure>
      </section>
    <sheetRequest/>
    </div>
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

<style lang="scss">
@import "./src/assets/scss/global.scss";

#app {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.logo {
  max-width:300px;
  margin: 20px 0;
}

.avatar-container {
    width: 320px;
    display: flex;
    justify-content: space-between;
    // border-bottom: 1px solid #236192;
    padding-bottom: 20px;
  img {
    border-radius: 100%;
    transform: scale(1);
    width: 70px;
    &:hover {
      transform: scale(1.1);
	    transition: .2s ease-in-out;
    }
  }
}
.page-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

figure {
  margin: 0;
  figcaption {
    font-size: 0.9em;
    text-align: center;
  }
}


</style>
