<template>
  <main class="content">
    <!-- Loop for the year collections -->
    <!-- Output the yearheading -->
    <table class="overall" v-for="table in yearCollections" v-bind:key="table.id">
      <thead>
        <tr>
          <th>{{ table.yearheading[0] }}</th>
        </tr>
        <tr>
          <th v-for="heading in table.tableheading" v-bind:key="heading.id">{{ heading }}</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="persons in table.names" v-bind:key="persons.id">
          <td v-for="entry in persons" v-bind:key="entry.id">{{ entry }}</td>
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
      selection: "/values/apisheet!A1:F13?",
      sheetData: "",
      currentYearData: "",
      previousYearsData: "",
      numberOfPeople: 4,
      numberOfYears: 2,
      yearCollections: []
    };
  },
  created() {
    this.init();
  },
  mounted() {},
  methods: {
    // Fetch data and assign it to sheetdata var
    async fetchData() {
      let response = await fetch(
        `${this.endpoint}${this.spreadsheet}${this.selection}$key=${
          this.apiKey
        }`
      );
      let data = await response.json();
      this.sheetData = data;
    },
    spliceJSONByYear() {
      let dataToBeSorted = this.sheetData.values;
      let yearCollectionContainer = [];

      // Counter for creating js object and adding it to
      let counter = 0;

      // Plus two, as it adds year heading and table row heading
      let totalMaxRows = (this.numberOfPeople + 2) * this.numberOfYears;

      // The amount of rows for each year collection
      let rowsPerCollection = totalMaxRows / this.numberOfYears;

      // Recursively slice arrays and create new year collection obj with each set
      function recursivelySliceArray(startingArray) {
        if (startingArray == totalMaxRows) {
          counter = 0;
          return;
        } else {
          let startingSlicePoint = startingArray;
          let endingSlicePoint = startingSlicePoint + rowsPerCollection;
          let slicedYearCollection = dataToBeSorted.slice(
            startingSlicePoint,
            endingSlicePoint
          );
          yearCollectionContainer[counter] = new yearCollection(
            slicedYearCollection
          );
          counter++;
          return recursivelySliceArray(endingSlicePoint);
        }
      }
      // Call the array with max total rows
      recursivelySliceArray(0);
      this.yearCollections = yearCollectionContainer;

      // Constructor to create year collections within container object
      function yearCollection(slicedJSONObject) {
        this.yearheading = slicedJSONObject[0];
        this.tableheading = slicedJSONObject[1];
        this.names = [];
        // Loop the names
        // In here sort the names by total win, also add in a prop for first, second third and 4th
        for (let e = 2; e < rowsPerCollection; e++) {
          this.names[e - 2] = slicedJSONObject[e];
        }
      }
    },
    sortedYearCollectionsByWinAmount() {
      let sortedYearCollections = this.yearCollections;
      console.log(sortedYearCollections);

      // Looping the year entries
      for (let entries of sortedYearCollections) {
        entries.names.sort(compare);
      }

      this.yearCollection = sortedYearCollections;

      // Compare and sort by total amount
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
      console.log(this.yearCollections);
    },
    async init() {
      await this.fetchData();
      this.spliceJSONByYear();
      this.sortedYearCollectionsByWinAmount();
      this.test();
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>

$color-primary:#6F263D;
$color-secondary:#236192;
$color-tertiary:#A7B1B7;

h1,h2,h3,h4,h5 {
  font-family: 'Raleway';
}
p, th, td {
  font-family: 'Raleway';
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
