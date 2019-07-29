<template>
  <div id="app">
    <div class="page-container">
      <h2></h2>
      <img alt="Lanchnation logo" src="./assets/img/logo.png" class="logo" />
      <section class="avatar-container">
        <figure>
          <img alt="Shep" src="./assets/img/shep.jpg" class="avatar" />
          <figcaption>Shep</figcaption>
          <div class="trophy-container">
            <img alt="Shep" src="./assets/svg/trophy.svg" class="trophy" />
          </div>
        </figure>
        <figure>
          <img alt="Hicks" src="./assets/img/hicks2.jpg" class="avatar" />
          <figcaption>Hicks</figcaption>
          <div class="trophy-container">
            <img alt="Shep" src="./assets/svg/silver.svg" class="trophy" />
          </div>
        </figure>
        <figure>
          <img alt="Jack" src="./assets/img/jack.jpg" class="avatar" />
          <figcaption>Jack</figcaption>
          <div class="trophy-container">
            <img alt="Third Place Medal" src="./assets/svg/bronze.svg" class="trophy" />
          </div>
        </figure>
        <figure>
          <img alt="Cragg" src="./assets/img/cragg.jpg" class="avatar" />
          <figcaption>Cragg</figcaption>
          <div class="trophy-container">
            <img alt="Shep" src="./assets/svg/hotdog.svg" class="trophy" />
          </div>
        </figure>
      </section>
      <sheetRequest />
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import sheetRequest from "./components/sheetRequest.vue";

export default {
  name: "app",
  components: {
    sheetRequest
  }
};
</script>

<style lang="scss">
@import "./src/assets/scss/global.scss";

#app {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.logo {
  max-width: 300px;
  margin: 20px 0;
}

.avatar-container {
  width: 320px;
  display: flex;
  justify-content: space-between;
  // border-bottom: 1px solid #236192;
  padding-bottom: 20px;
  img.avatar {
    border-radius: 100%;
    transform: scale(1);
    width: 70px;
    &:hover {
      transform: scale(1.1);
      transition: 0.2s ease-in-out;
    }
  }
  img.trophy {
    height: 20px;
    text-align: center;
  }
}
.page-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

figure {
  margin: 0;
  background-color: #2c3033;
  color: white;
  box-shadow: 5px 5px #88888831;
  figcaption {
    font-size: 0.9em;
    text-align: center;
  }
}

.trophy-container {
  text-align: center;
}
</style>
