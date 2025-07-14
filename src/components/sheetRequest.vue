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

      <h2>All Time Winnings</h2>
      <table v-if="allTimeWinnings.length">
        <thead>
          <tr>
            <th>Rank</th>
            <th>Name</th>
            <th>Total Winnings</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(row, idx) in allTimeWinnings" :key="row.name">
            <td>{{ idx + 1 }}</td>
            <td>{{ row.name }}</td>
            <td>£{{ row.total.toLocaleString(undefined, {minimumFractionDigits: 2, maximumFractionDigits: 2}) }}</td>
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
        { key: "2024", label: "2024/25", selection: "/values/2024apisheet!A1:B10?" },
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
    allTimeWinnings() {
      // Aggregate all previous years' winnings by name
      const totals = {};
      for (const year of this.previousYears) {
        const data = this.yearData[year.key];
        if (data && Array.isArray(data.names)) {
          data.names.forEach(row => {
            const name = row[0];
            // Remove pound sign and commas, parse as float
            const amount = parseFloat((row[1] || '').replace(/[^\d.-]/g, ''));
            if (!isNaN(amount)) {
              if (!totals[name]) totals[name] = 0;
              totals[name] += amount;
            }
          });
        }
      }
      // Convert to array and sort by total descending
      return Object.entries(totals)
        .map(([name, total]) => ({ name, total }))
        .sort((a, b) => b.total - a.total);
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
