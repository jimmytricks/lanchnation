<template>
  <main class="content">
    <table class="overall">
      <thead>
        <tr>
          <th colspan="2" v-if="currentYear">{{ currentYear.yearheading[0] }}</th>
        </tr>
        <tr>
          <th v-for="heading in currentYear.tableheading" v-bind:key="heading.id">{{ heading }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="persons in currentYear.names" v-bind:key="persons.id">
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

    <h2>In play bets</h2>

    <h2>Previous Years</h2>

    <table class="overall">
      <thead>
        <tr>
          <th colspan="2" v-if="year2018">{{ year2018.yearheading[0] }}</th>
        </tr>
        <tr>
          <th v-for="heading in year2018.tableheading" v-bind:key="heading.id">{{ heading }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="persons in year2018.names" v-bind:key="persons.id">
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
      currentSheetData: "",
      sheetData2018: "",
      currentYearData: "",
      previousYearsData: "",
      numberOfPeople: 4,
      numberOfYears: 2,
      numberOfHeadings: 2,
      numberOfHeadingsAndPeople: 6,
      currentYear: "",
      year2018: "",
      yearCollections: []
    };
  },
  created() {
    this.initCurrentYear();
    this.init2018();
  },
  mounted() {},
  methods: {
    // Fetch data and assign it to sheetdata var
    async fetchData(yearSelect, specificYear) {
      let response = await fetch(
        `${this.endpoint}${this.spreadsheet}${yearSelect}$key=${this.apiKey}`
      );
      let data = await response.json();

      if (specificYear == "currentSheetData") {
        this.currentSheetData = data;
      } else {
        this.sheetData2018 = data;
      }
    },
    createTable(specificYear) {
      let tableData;
      if (specificYear == "currentSheetData") {
        tableData = this.currentSheetData.values;
      } else {
        tableData = this.sheetData2018.values;
      }

      let tableObj = {};
      // Constructor to create year collections within container object

      const yearCollection = tableData => {
        tableObj.yearheading = tableData[0];
        tableObj.tableheading = tableData[1];
        tableObj.names = [];
        tableObj.details = [];

        // Loop the names
        // In here sort the names by total win, also add in a prop for first, second third and 4th
        for (
          let e = this.numberOfHeadings;
          e < this.numberOfHeadings + this.numberOfPeople;
          e++
        ) {
          tableObj.names[e - this.numberOfHeadings] = tableData[e];
        }

        // ***** need to change for loop into a substring / filter that gets the remaining values as an array
        for (
          let e = 0;
          e < tableData.length - this.numberOfHeadingsAndPeople;
          e++
        ) {
          tableObj.details[e] = tableData[e + this.numberOfHeadingsAndPeople];
        }

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
    async initCurrentYear() {
      await this.fetchData(this.currentYearSelection, "currentSheetData");
      this.createTable("currentSheetData");
      this.sortedYearCollectionsByWinAmount("currentSheetData");
    },
    async init2018() {
      await this.fetchData(this.yearSelection2018, "sheetData2018");
      this.createTable("sheetData2018");
      this.sortedYearCollectionsByWinAmount("sheetData2018");
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
$color-primary: #6f263d;
$color-secondary: #236192;
$color-tertiary: #a7b1b7;

h1,
h2,
h3,
h4,
h5 {
  font-family: "Raleway";
}
p,
th,
td {
  font-family: "Raleway";
}

/* Table styling */

table {
  margin-bottom: 20px;
  color: white;
  border-spacing: 0;
  th {
    padding: 10px 20px;
    background-color: $color-secondary;
  }
  td {
    background-color: $color-primary;
    padding: 10px 20px;
  }
}
</style>
