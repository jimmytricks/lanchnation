<template>
  <div id="app">
    <div class="page-container">
      <img alt="Lanchnation logo" src="./assets/img/logo.png" class="logo" />
      <h2 class="leaderboard">Leaderboard</h2>
      <section class="avatar-container">
        <figure v-for="person in leaderboard" :key="person.name">
          <img :alt="`Avatar of ${person.name}`" :src="person.avatar" class="avatar" />
          <figcaption>{{ person.name }}</figcaption>
          <div class="trophy-container">
            <img v-for="(trophy, i) in person.trophies" :key="i" :alt="trophy.alt" :src="trophy.src" class="trophy" />
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
  async mounted() {
    await this.$nextTick();
    const sheet = this.$children.find(c => c.$options.name === "sheetRequest");
    if (!sheet) return;
    const years = sheet.previousYears.map(y => y.key);
    const yearData = sheet.yearData;
    const trophyMap = { 0: TROPHIES[0], 1: TROPHIES[1], 2: TROPHIES[2], 3: TROPHIES[3] };
    const personTrophies = { Shep: [], Hicks: [], Jack: [], Cryan: [] };
    for (const year of years) {
      const standings = yearData[year]?.names || [];
      standings.forEach((row, idx) => {
        const name = row[0];
        if (personTrophies[name] && trophyMap[idx]) {
          personTrophies[name].push(trophyMap[idx]);
        }
      });
    }
    // Update leaderboard
    this.leaderboard.forEach(person => {
      person.trophies = personTrophies[person.name];
    });
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
