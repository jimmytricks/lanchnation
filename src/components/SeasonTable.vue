<template>
  <section>
    <table>
      <thead>
        <tr>
          <th :colspan="standings.tableheading.length + 1">
            {{ standings.yearheading[0] }}
          </th>
        </tr>
        <tr>
          <th>Pos.</th>
          <th v-for="heading in standings.tableheading" :key="heading">
            {{ heading }}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(persons, index) in standings.names" :key="index">
          <td>
            <img v-if="index < 4" :alt="trophyAlt(index)" :src="trophySrc(index)" class="trophy" />
          </td>
          <td v-for="(entry, i) in persons" :key="i">{{ entry }}</td>
        </tr>
      </tbody>
    </table>
    <table>
      <thead>
        <tr>
          <th colspan="2">{{ standings.yearheading[0] }} Awards</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(details, i) in standings.details" :key="i">
          <td v-for="(entry, j) in details" :key="j" class="season-details">{{ entry }}</td>
        </tr>
      </tbody>
    </table>
  </section>
</template>

<script>
export default {
  name: "SeasonTable",
  props: {
    standings: { type: Object, required: true },
  },
  methods: {
    trophySrc(index) {
      const icons = [
        require("../assets/svg/trophy.svg"),
        require("../assets/svg/silver.svg"),
        require("../assets/svg/bronze.svg"),
        require("../assets/svg/hotdog.svg")
      ];
      return icons[index];
    },
    trophyAlt(index) {
      const alts = [
        "First Place",
        "Second Place",
        "Third Place",
        "Last Place"
      ];
      return alts[index];
    },
  },
};
</script>

<style scoped>
.trophy {
  height: 20px;
}
</style>
