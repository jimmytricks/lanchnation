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
      currentYearSelection: "/values/currentapisheet!A1:B11?",
      currentSheetData: "",
      currentYearData: "",
      previousYearsData: "",
      numberOfPeople: 4,
      numberOfYears: 2,
      numberOfHeadings: 2,
      numberOfHeadingsAndPeople: 6,
      currentYear: "",
      yearCollections: []
    };
  },
  created() {
    this.init();
  },
  mounted() {},
  methods: {
    // Fetch data and assign it to sheetdata var
    async fetchData(yearSelect, vuePropToSaveItTo) {
      console.log(vuePropToSaveItTo);
      let response = await fetch(
        `${this.endpoint}${this.spreadsheet}${yearSelect}$key=${this.apiKey}`
      );
      let data = await response.json();
      this.currentSheetData = data;
    },
    createTable() {
      let tableData = this.currentSheetData.values;
      const tableObj = {};
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
          console.log("test");
          tableObj.details[e] = tableData[e + this.numberOfHeadingsAndPeople];
        }
        console.log(tableObj.details);
        this.currentYear = tableObj;
      };

      yearCollection(tableData);
    },
    // spliceJSONByYear() {
    //   let dataToBeSorted = this.sheetData.values;
    //   let yearCollectionContainer = [];

    //   // Counter for creating js object and adding it to
    //   let counter = 0;

    //   // Plus two, as it adds year heading and table row heading
    //   let totalMaxRows = (this.numberOfPeople + 2) * this.numberOfYears;

    //   // The amount of rows for each year collection
    //   let rowsPerCollection = totalMaxRows / this.numberOfYears;

    //   // Recursively slice arrays and create new year collection obj with each set
    //   function recursivelySliceArray(startingArray) {
    //     if (startingArray == totalMaxRows) {
    //       counter = 0;
    //       return;
    //     } else {
    //       let startingSlicePoint = startingArray;
    //       let endingSlicePoint = startingSlicePoint + rowsPerCollection;
    //       let slicedYearCollection = dataToBeSorted.slice(
    //         startingSlicePoint,
    //         endingSlicePoint
    //       );
    //       yearCollectionContainer[counter] = new yearCollection(
    //         slicedYearCollection
    //       );
    //       counter++;
    //       return recursivelySliceArray(endingSlicePoint);
    //     }
    //   }
    //   // Call the array with max total rows
    //   recursivelySliceArray(0);
    //   this.yearCollections = yearCollectionContainer;

    //   // Constructor to create year collections within container object
    //   function yearCollection(slicedJSONObject) {
    //     this.yearheading = slicedJSONObject[0];
    //     this.tableheading = slicedJSONObject[1];
    //     this.names = [];
    //     // Loop the names
    //     // In here sort the names by total win, also add in a prop for first, second third and 4th
    //     for (let e = 2; e < rowsPerCollection; e++) {
    //       this.names[e - 2] = slicedJSONObject[e];
    //     }
    //   }
    // },
    sortedYearCollectionsByWinAmount() {
      this.currentYear.names.sort(compare);

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
    test() {
      // console.log(this.yearCollections);
      console.log(this.headingsAndPeople);
    },
    async init() {
      await this.fetchData(this.currentYearSelection, 'currentSheetData');
      this.createTable();
      this.sortedYearCollectionsByWinAmount();
      this.test();
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
