<template>
  <main class="content">
    <section class="table-container">
      <h2>Current Season</h2>
      <table class="overall">
        <thead>
          <tr>
            <th colspan="3" v-if="currentYear">{{ currentYear.yearheading[0] }}</th>
          </tr>
          <tr>
            <th>Pos.</th>
            <th v-for="heading in currentYear.tableheading" v-bind:key="heading.id">{{ heading }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(persons, index) in currentYear.names" v-bind:key="persons.id">
            <td>{{ index + 1 }}.</td>
            <td v-for="entry in persons" v-bind:key="entry.id">{{ entry }}</td>
          </tr>
        </tbody>
      </table>

      <table class="overall">
        <thead>
          <tr>
            <th colspan="2" v-if="currentYear">Current Year Awards</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="details in currentYear.details" v-bind:key="details.id">
            <td
              v-for="entrydetails in details"
              v-bind:key="entrydetails.id"
              class="season-details"
            >{{ entrydetails }}</td>
          </tr>
        </tbody>
      </table>

      <h2 class="inplay">In-play bets</h2>

      <table class="inplay">
        <thead>
          <tr>
            <th
              v-for="heading in inplayDataToDisplay.tableheading"
              v-bind:key="heading.id"
            >{{ heading }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="entries in inplayDataToDisplay.details" v-bind:key="entries.id">
            <td v-for="entry in entries" v-bind:key="entry.id">{{ entry }}</td>
          </tr>
        </tbody>
      </table>

      <h2>Previous Years</h2>

      <table class="overall">
        <thead>
          <tr>
            <th colspan="3" v-if="year2018">{{ year2018.yearheading[0] }}</th>
          </tr>
          <tr>
            <th>Pos.</th>
            <th v-for="heading in year2018.tableheading" v-bind:key="heading.id">{{ heading }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(persons, index) in year2018.names" v-bind:key="persons.id">
            <td v-if="index == 0"><img alt="First Place" src="../assets/svg/trophy.svg" class="trophy" /></td>
            <td v-else-if="index == 1"><img alt="Second Place" src="../assets/svg/silver.svg" class="trophy" /></td>
            <td v-else-if="index == 2"><img alt="Third Place" src="../assets/svg/bronze.svg" class="trophy" /></td>
            <td v-else-if="index == 3"><img alt="Last Place" src="../assets/svg/hotdog.svg" class="trophy" /></td>
            <td v-for="entry in persons" v-bind:key="entry.id">{{ entry }}</td>
          </tr>
        </tbody>
      </table>

      <table class="overall">
        <thead>
          <tr>
            <th colspan="2" v-if="currentYear">2018/2019 Awards</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="details in year2018.details" v-bind:key="details.id">
            <td
              v-for="entrydetails in details"
              v-bind:key="entrydetails.id"
              class="season-details"
            >{{ entrydetails }}</td>
          </tr>
        </tbody>
      </table>
    </section>
  </main>
</template>

<script>
/* eslint-disable */
export default {
  name: "sheetRequest",
  props: {
    msg: String
  },
  data() {
    return {
      endpoint: "https://sheets.googleapis.com/v4/spreadsheets/",
      spreadsheet: process.env.VUE_APP_SPREADSHEETID,
      apiKey: process.env.VUE_APP_GOOGLE,
      currentYearSelection: "/values/currentapisheet!A1:B10?",
      yearSelection2018: "/values/2018apisheet!A1:B10?",
      inplaySelection: "/values/inplay!A1:C20?",
      currentSheetData: "",
      sheetData2018: "",
      inplayData: "",
      numberOfPeople: 4,
      numberOfYears: 2,
      numberOfHeadings: 2,
      numberOfHeadingsAndPeople: 6,
      currentYear: "",
      year2018: "",
      inplayDataToDisplay: ""
    };
  },
  created() {
    this.initCurrentYear();
    this.init2018();
    this.initInplay();
  },
  mounted() {},
  methods: {
    // Fetch data and assign it to sheetdata var
    async fetchData(yearSelect, sheetSelection) {
      let response = await fetch(
        `${this.endpoint}${this.spreadsheet}${yearSelect}$key=${this.apiKey}`
      );
      let data = await response.json();

      if (sheetSelection == "currentSheetData") {
        this.currentSheetData = data;
      } else if (sheetSelection == "sheetData2018") {
        this.sheetData2018 = data;
      } else {
        this.inplayData = data;
      }
    },
    createResultsTable(specificYear) {
      let tableData;
      if (specificYear == "currentSheetData") {
        tableData = this.currentSheetData.values;
      } else {
        tableData = this.sheetData2018.values;
      }

      let tableObj = {};

      const yearCollection = tableData => {
        tableObj.yearheading = tableData[0];
        tableObj.tableheading = tableData[1];
        tableObj.names = tableData.slice(
          this.numberOfHeadings,
          this.numberOfHeadingsAndPeople
        );
        tableObj.details = tableData.slice(this.numberOfHeadingsAndPeople);

        if (specificYear == "currentSheetData") {
          this.currentYear = tableObj;
        } else {
          this.year2018 = tableObj;
        }
      };

      yearCollection(tableData);
    },
    sortedYearCollectionsByWinAmount(specificYear) {
      if (specificYear == "currentSheetData") {
        this.currentYear.names.sort(compare);
      } else {
        this.year2018.names.sort(compare);
      }

      function compare(a, b) {
        if (parseFloat(a[1]) > parseFloat(b[1])) {
          return -1;
        }
        if (a[1 < b[1]]) {
          return 1;
        }
        return 0;
      }
    },
    createInplayTable() {
      let tableData = this.inplayData.values;

      let tableObj = {};
      tableObj.tableheading = tableData[0];
      tableObj.details = tableData.slice(1);

      this.inplayDataToDisplay = tableObj;
      console.log(this.inplayDataToDisplay);
    },
    async initCurrentYear() {
      await this.fetchData(this.currentYearSelection, "currentSheetData");
      this.createResultsTable("currentSheetData");
      this.sortedYearCollectionsByWinAmount("currentSheetData");
    },
    async init2018() {
      await this.fetchData(this.yearSelection2018, "sheetData2018");
      this.createResultsTable("sheetData2018");
      this.sortedYearCollectionsByWinAmount("sheetData2018");
    },
    async initInplay() {
      await this.fetchData(this.inplaySelection, "inplay");
      this.createInplayTable();
    }
  }
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
