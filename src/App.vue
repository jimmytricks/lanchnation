<template>
  <div id="app">
    <div class="page-container">
      <img alt="Lanchnation logo" src="./assets/img/logo.png" class="logo" />
      <h2 class="leaderboard">Leaderboard</h2>
      <section class="avatar-container">
        <figure>
          <img alt="Avatar of Shep" src="./assets/img/shep.jpg" class="avatar" />
          <figcaption>Shep</figcaption>
          <div class="trophy-container">
            <img alt="First Place" src="./assets/svg/trophy.svg" class="trophy" />
            <img alt="First Place" src="./assets/svg/trophy.svg" class="trophy" />
            <img alt="Hotdog for last place" src="./assets/svg/hotdog.svg" class="trophy" />
            <img alt="First Place" src="./assets/svg/trophy.svg" class="trophy" />
            <img alt="First Place" src="./assets/svg/trophy.svg" class="trophy" />
            <img alt="Third Place Medal" src="./assets/svg/bronze.svg" class="trophy" />
            <img alt="First Place" src="./assets/svg/trophy.svg" class="trophy" />
          </div>
        </figure>
        <figure>
          <img alt="Avatar of Hicks" src="./assets/img/hicks2.jpg" class="avatar" />
          <figcaption>Hicks</figcaption>
          <div class="trophy-container">
            <img alt="Second place" src="./assets/svg/silver.svg" class="trophy" />
            <img alt="Second place" src="./assets/svg/silver.svg" class="trophy" />
            <img alt="Third Place Medal" src="./assets/svg/bronze.svg" class="trophy" />
            <img alt="Third Place Medal" src="./assets/svg/bronze.svg" class="trophy" />
            <img alt="Hotdog for last place" src="./assets/svg/hotdog.svg" class="trophy" />
            <img alt="Hotdog for last place" src="./assets/svg/hotdog.svg" class="trophy" />
            <img alt="Second place" src="./assets/svg/silver.svg" class="trophy" />
          </div>
        </figure>
        <figure>
          <img alt="Avatar of Jack" src="./assets/img/jack.jpg" class="avatar" />
          <figcaption>Jack</figcaption>
          <div class="trophy-container">
            <img alt="Third Place Medal" src="./assets/svg/bronze.svg" class="trophy" />
            <img alt="Third Place Medal" src="./assets/svg/bronze.svg" class="trophy" />
            <img alt="Second place" src="./assets/svg/silver.svg" class="trophy" />
            <img alt="Second place" src="./assets/svg/silver.svg" class="trophy" />
            <img alt="Third Place Medal" src="./assets/svg/bronze.svg" class="trophy" />
            <img alt="Second place" src="./assets/svg/silver.svg" class="trophy" />
            <img alt="Third Place Medal" src="./assets/svg/bronze.svg" class="trophy" />
          </div>
        </figure>
        <figure>
          <img alt="Avatar of Cragg" src="./assets/img/cragg.jpg" class="avatar" />
          <figcaption>Cryan</figcaption>
          <div class="trophy-container">
            <img alt="Hotdog for last place" src="./assets/svg/hotdog.svg" class="trophy" />
            <img alt="Hotdog for last place" src="./assets/svg/hotdog.svg" class="trophy" />
            <img alt="First Place" src="./assets/svg/trophy.svg" class="trophy" />
            <img alt="Hotdog for last place" src="./assets/svg/hotdog.svg" class="trophy" />
            <img alt="Second place" src="./assets/svg/silver.svg" class="trophy" />
            <img alt="First Place" src="./assets/svg/trophy.svg" class="trophy" />
            <img alt="Hotdog for last place" src="./assets/svg/hotdog.svg" class="trophy" />
          </div>
        </figure>
      </section>
      <sheetRequest />
    </div>
  </div>
</template>

<script>
import sheetRequest from "./components/sheetRequest.vue";

const AVATARS = {
  Shep: require("./assets/img/shep.jpg"),
  Hicks: require("./assets/img/hicks2.jpg"),
  Jack: require("./assets/img/jack.jpg"),
  Cryan: require("./assets/img/cragg.jpg")
};
const TROPHIES = [
  { alt: "First Place", src: require("./assets/svg/trophy.svg") },
  { alt: "Second Place", src: require("./assets/svg/silver.svg") },
  { alt: "Third Place", src: require("./assets/svg/bronze.svg") },
  { alt: "Last Place", src: require("./assets/svg/hotdog.svg") }
];

export default {
  name: "app",
  components: {
    sheetRequest
  },
  data() {
    return {
      leaderboard: [
        { name: "Shep", avatar: AVATARS.Shep, trophies: [] },
        { name: "Hicks", avatar: AVATARS.Hicks, trophies: [] },
        { name: "Jack", avatar: AVATARS.Jack, trophies: [] },
        { name: "Cryan", avatar: AVATARS.Cryan, trophies: [] }
      ]
    };
  },
  mounted() {
    this.waitForSheetDataAndAssignTrophies();
  },
  methods: {
    async waitForSheetDataAndAssignTrophies() {
      let sheet = null;
      while (!sheet) {
        await this.$nextTick();
        sheet = this.$children.find(c => c.$options.name === "sheetRequest");
      }
      let tries = 0;
      const allYears = sheet.years.map(y => y.key); 
      while (tries < 50) { 
        const ready = allYears.every(y => {
          const d = sheet.yearData[y];
          return d && Array.isArray(d.names);
        });
        if (ready) break;
        await new Promise(res => setTimeout(res, 100));
        tries++;
      }
      const yearData = sheet.yearData;
      const trophyMap = { 0: TROPHIES[0], 1: TROPHIES[1], 2: TROPHIES[2], 3: TROPHIES[3] };
      const personTrophies = { Shep: [], Hicks: [], Jack: [], Cryan: [] };
      for (const year of allYears) {
        const standings = (yearData[year] && Array.isArray(yearData[year].names)) ? yearData[year].names : [];
        standings.forEach((row, idx) => {
          const name = row[0];
          if (personTrophies[name] && trophyMap[idx]) {
            personTrophies[name].push(trophyMap[idx]);
          }
        });
      }
      this.leaderboard.forEach(person => {
        person.trophies = personTrophies[person.name];
      });
    }
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
  margin: 4px 0 16px 0;
}

.avatar-container {
  width: 320px;
  display: flex;
  justify-content: space-between;
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
    padding: 5px;
  }
}

.trophy-container {
  text-align: center;
  padding: 4px 2px;

}

h2 {
  margin-top: 0;
  margin-bottom: 10px;
  padding: 8px 0;
  color: #2c3033;
  letter-spacing: 1.4px;
  text-transform: uppercase;
  font-size: 1em;
  background-color: #2c3033;
  width: 320px;
  color: white;
  text-align: center;
  box-shadow: 5px 5px #88888831;
  margin-top: 20px;
}

h2.leaderboard {
  margin-top: 0;
}

// Media queries

@media (min-width: 640px) {
  .avatar-container, h2 {
    width: 480px;
  }
  figure {
    padding: 10px;
  }
  .avatar-container img.avatar {
    width: 90px;
  }
}
</style>
