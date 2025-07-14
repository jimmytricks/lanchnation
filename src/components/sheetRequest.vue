<template>
  <main class="content">
    <section class="table-container">
      <h2>Current Season</h2>
      <SeasonTable v-if="yearData.current" :standings="yearData.current" />

      <h2 class="inplay">In-play bets</h2>
      <table class="inplay" v-if="inplayDataToDisplay">
        <thead>
          <tr>
            <th v-for="heading in inplayDataToDisplay.tableheading" :key="heading">
              {{ heading }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(entries, i) in inplayDataToDisplay.details" :key="i">
            <td v-for="(entry, j) in entries" :key="j">{{ entry }}</td>
          </tr>
        </tbody>
      </table>

      <h2>Previous Years</h2>
      <SeasonTable
        v-for="year in previousYears"
        :key="year.key"
        v-if="yearData[year.key]"
        :standings="yearData[year.key]"
      />
    </section>
  </main>
</template>

<script>
import SeasonTable from "./SeasonTable.vue";

export default {
  name: "sheetRequest",
  components: { SeasonTable },
  data() {
    return {
      endpoint: "https://sheets.googleapis.com/v4/spreadsheets/",
      spreadsheet: process.env.VUE_APP_SPREADSHEETID,
      apiKey: process.env.VUE_APP_GOOGLE,
      years: [
        { key: "current", label: "Current Year 2023/24", selection: "/values/currentapisheet!A1:B10?" },
        { key: "2023", label: "2023/24", selection: "/values/2023apisheet!A1:B10?" },
        { key: "2022", label: "2022/23", selection: "/values/2022apisheet!A1:B10?" },
        { key: "2021", label: "2021/22", selection: "/values/2021apisheet!A1:B10?" },
        { key: "2020", label: "2020/21", selection: "/values/2020apisheet!A1:B10?" },
        { key: "2019", label: "2019/20", selection: "/values/2019apisheet!A1:B10?" },
        { key: "2018", label: "2018/19", selection: "/values/2018apisheet!A1:B10?" },
      ],
      inplaySelection: "/values/inplay!A1:C60?",
      yearData: {},
      inplayData: "",
      inplayDataToDisplay: "",
      numberOfPeople: 4,
      numberOfHeadings: 2,
      numberOfHeadingsAndPeople: 6,
    };
  },
  computed: {
    previousYears() {
      // Exclude 'current' from previous years
      return this.years.filter(y => y.key !== "current");
    },
  },
  async created() {
    await this.initAllYears();
    await this.initInplay();
  },
  methods: {
    async fetchData(yearSelect) {
      const response = await fetch(
        `${this.endpoint}${this.spreadsheet}${yearSelect}key=${this.apiKey}`
      ).then((response) => response.json());
      return response;
    },
    parseYearData(tableData) {
      const tableObj = {};
      tableObj.yearheading = tableData[0];
      tableObj.tableheading = tableData[1];
      tableObj.names = tableData.slice(this.numberOfHeadings, this.numberOfHeadingsAndPeople);
      tableObj.details = tableData.slice(this.numberOfHeadingsAndPeople);
      // Sort and add pound sign
      tableObj.names = tableObj.names.sort((a, b) => parseFloat(b[1]) - parseFloat(a[1]))
        .map(row => [row[0], `£${row[1]}`]);
      return tableObj;
    },
    async initAllYears() {
      for (const year of this.years) {
        const data = await this.fetchData(year.selection);
        if (data && data.values) {
          this.$set(this.yearData, year.key, this.parseYearData(data.values));
        }
      }
    },
    async initInplay() {
      const data = await this.fetchData(this.inplaySelection);
      if (data && data.values) {
        const tableObj = {};
        tableObj.tableheading = data.values[0];
        tableObj.details = data.values.slice(1);
        this.inplayDataToDisplay = tableObj;
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
@import "./src/assets/scss/global.scss";

/* Table styling */

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
