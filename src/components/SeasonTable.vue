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

<style lang="scss">
@import "./src/assets/scss/global.scss";

/* Table styling */

.trophy {
  height: 20px;
}

.table-container {
  display: flex;
  flex-direction: column;
  align-items: center;
}

table {
  min-width: 320px;
  margin-bottom: 20px;
  color: white;
  border-spacing: 0;
  box-shadow: 5px 5px #88888831;
  th {
    padding: 10px 20px;
    background-color: $color-secondary;
    border-bottom: 1px solid #5c88ab;
    text-align: left;
  }
  td {
    background-color: $color-primary;
    padding: 10px 20px;
    border-bottom: 1px solid rgba(179, 128, 144, 0.65);
    text-align: left;
  }
}

.inplay {
  tr:last-child {
    td {
      background-color: rgba($color-primary, 0.9);
    }
  }
}

h2.inplay {
  width: 100%;
}

.trophy {
  height: 20px;
}

// Media queries

@media (min-width: 640px) {
  table {
    min-width: 480px;
  }
}
</style>
