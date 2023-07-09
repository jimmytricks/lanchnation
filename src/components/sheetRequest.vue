<template>
  <main class="content">
    <section class="table-container">
      <h2>Current Season</h2>
      <table>
        <thead>
          <tr>
            <th colspan="3">
              Current Year 2021/22
            </th>
          </tr>
          <tr>
            <th>Pos.</th>
            <th
              v-for="heading in currentYearDataToDisplay.tableheading"
              v-bind:key="heading.id"
            >
              {{ heading }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(persons, index) in currentYearDataToDisplay.names"
            v-bind:key="persons.id"
          >
            <td>{{ index + 1 }}.</td>
            <td v-for="entry in persons" v-bind:key="entry.id">{{ entry }}</td>
          </tr>
        </tbody>
      </table>

      <table>
        <thead>
          <tr>
            <th colspan="2" v-if="currentYearDataToDisplay">
              Current Year Awards
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="details in currentYearDataToDisplay.details"
            v-bind:key="details.id"
          >
            <td
              v-for="entrydetails in details"
              v-bind:key="entrydetails.id"
              class="season-details"
            >
              {{ entrydetails }}
            </td>
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
            >
              {{ heading }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="entries in inplayDataToDisplay.details"
            v-bind:key="entries.id"
          >
            <td v-for="entry in entries" v-bind:key="entry.id">{{ entry }}</td>
          </tr>
        </tbody>
      </table>

      <h2>Previous Years</h2>

      <table>
        <thead>
          <tr>
            <th colspan="3" v-if="year2022DataToDisplay">
              {{ year2022DataToDisplay.yearheading[0] }}
            </th>
          </tr>
          <tr>
            <th>Pos.</th>
            <th
              v-for="heading in year2022DataToDisplay.tableheading"
              v-bind:key="heading.id"
            >
              {{ heading }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(persons, index) in year2022DataToDisplay.names"
            v-bind:key="persons.id"
          >
            <td v-if="index == 0">
              <img
                alt="First Place"
                src="../assets/svg/trophy.svg"
                class="trophy"
              />
            </td>
            <td v-else-if="index == 1">
              <img
                alt="Second Place"
                src="../assets/svg/silver.svg"
                class="trophy"
              />
            </td>
            <td v-else-if="index == 2">
              <img
                alt="Third Place"
                src="../assets/svg/bronze.svg"
                class="trophy"
              />
            </td>
            <td v-else-if="index == 3">
              <img
                alt="Last Place"
                src="../assets/svg/hotdog.svg"
                class="trophy"
              />
            </td>
            <td v-for="entry in persons" v-bind:key="entry.id">{{ entry }}</td>
          </tr>
        </tbody>
      </table>

      <table>
        <thead>
          <tr>
            <th colspan="2" v-if="currentYearDataToDisplay">2022/23 Awards</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="details in year2022DataToDisplay.details"
            v-bind:key="details.id"
          >
            <td
              v-for="entrydetails in details"
              v-bind:key="entrydetails.id"
              class="season-details"
            >
              {{ entrydetails }}
            </td>
          </tr>
        </tbody>
      </table>

      <table>
        <thead>
          <tr>
            <th colspan="3" v-if="year2021DataToDisplay">
              {{ year2021DataToDisplay.yearheading[0] }}
            </th>
          </tr>
          <tr>
            <th>Pos.</th>
            <th
              v-for="heading in year2021DataToDisplay.tableheading"
              v-bind:key="heading.id"
            >
              {{ heading }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(persons, index) in year2021DataToDisplay.names"
            v-bind:key="persons.id"
          >
            <td v-if="index == 0">
              <img
                alt="First Place"
                src="../assets/svg/trophy.svg"
                class="trophy"
              />
            </td>
            <td v-else-if="index == 1">
              <img
                alt="Second Place"
                src="../assets/svg/silver.svg"
                class="trophy"
              />
            </td>
            <td v-else-if="index == 2">
              <img
                alt="Third Place"
                src="../assets/svg/bronze.svg"
                class="trophy"
              />
            </td>
            <td v-else-if="index == 3">
              <img
                alt="Last Place"
                src="../assets/svg/hotdog.svg"
                class="trophy"
              />
            </td>
            <td v-for="entry in persons" v-bind:key="entry.id">{{ entry }}</td>
          </tr>
        </tbody>
      </table>

      <table>
        <thead>
          <tr>
            <th colspan="2" v-if="currentYearDataToDisplay">2021/22 Awards</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="details in year2021DataToDisplay.details"
            v-bind:key="details.id"
          >
            <td
              v-for="entrydetails in details"
              v-bind:key="entrydetails.id"
              class="season-details"
            >
              {{ entrydetails }}
            </td>
          </tr>
        </tbody>
      </table>

      <table>
        <thead>
          <tr>
            <th colspan="3" v-if="year2020DataToDisplay">
              {{ year2020DataToDisplay.yearheading[0] }}
            </th>
          </tr>
          <tr>
            <th>Pos.</th>
            <th
              v-for="heading in year2020DataToDisplay.tableheading"
              v-bind:key="heading.id"
            >
              {{ heading }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(persons, index) in year2020DataToDisplay.names"
            v-bind:key="persons.id"
          >
            <td v-if="index == 0">
              <img
                alt="First Place"
                src="../assets/svg/trophy.svg"
                class="trophy"
              />
            </td>
            <td v-else-if="index == 1">
              <img
                alt="Second Place"
                src="../assets/svg/silver.svg"
                class="trophy"
              />
            </td>
            <td v-else-if="index == 2">
              <img
                alt="Third Place"
                src="../assets/svg/bronze.svg"
                class="trophy"
              />
            </td>
            <td v-else-if="index == 3">
              <img
                alt="Last Place"
                src="../assets/svg/hotdog.svg"
                class="trophy"
              />
            </td>
            <td v-for="entry in persons" v-bind:key="entry.id">{{ entry }}</td>
          </tr>
        </tbody>
      </table>

      <table>
        <thead>
          <tr>
            <th colspan="2" v-if="currentYearDataToDisplay">2020/21 Awards</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="details in year2020DataToDisplay.details"
            v-bind:key="details.id"
          >
            <td
              v-for="entrydetails in details"
              v-bind:key="entrydetails.id"
              class="season-details"
            >
              {{ entrydetails }}
            </td>
          </tr>
        </tbody>
      </table>

      <table>
        <thead>
          <tr>
            <th colspan="3" v-if="year2019DataToDisplay">
              {{ year2019DataToDisplay.yearheading[0] }}
            </th>
          </tr>
          <tr>
            <th>Pos.</th>
            <th
              v-for="heading in year2019DataToDisplay.tableheading"
              v-bind:key="heading.id"
            >
              {{ heading }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(persons, index) in year2019DataToDisplay.names"
            v-bind:key="persons.id"
          >
            <td v-if="index == 0">
              <img
                alt="First Place"
                src="../assets/svg/trophy.svg"
                class="trophy"
              />
            </td>
            <td v-else-if="index == 1">
              <img
                alt="Second Place"
                src="../assets/svg/silver.svg"
                class="trophy"
              />
            </td>
            <td v-else-if="index == 2">
              <img
                alt="Third Place"
                src="../assets/svg/bronze.svg"
                class="trophy"
              />
            </td>
            <td v-else-if="index == 3">
              <img
                alt="Last Place"
                src="../assets/svg/hotdog.svg"
                class="trophy"
              />
            </td>
            <td v-for="entry in persons" v-bind:key="entry.id">{{ entry }}</td>
          </tr>
        </tbody>
      </table>

      <table>
        <thead>
          <tr>
            <th colspan="2" v-if="currentYearDataToDisplay">2019/20 Awards</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="details in year2019DataToDisplay.details"
            v-bind:key="details.id"
          >
            <td
              v-for="entrydetails in details"
              v-bind:key="entrydetails.id"
              class="season-details"
            >
              {{ entrydetails }}
            </td>
          </tr>
        </tbody>
      </table>

      <table>
        <thead>
          <tr>
            <th colspan="3" v-if="year2018DataToDisplay">
              {{ year2018DataToDisplay.yearheading[0] }}
            </th>
          </tr>
          <tr>
            <th>Pos.</th>
            <th
              v-for="heading in year2018DataToDisplay.tableheading"
              v-bind:key="heading.id"
            >
              {{ heading }}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(persons, index) in year2018DataToDisplay.names"
            v-bind:key="persons.id"
          >
            <td v-if="index == 0">
              <img
                alt="First Place"
                src="../assets/svg/trophy.svg"
                class="trophy"
              />
            </td>
            <td v-else-if="index == 1">
              <img
                alt="Second Place"
                src="../assets/svg/silver.svg"
                class="trophy"
              />
            </td>
            <td v-else-if="index == 2">
              <img
                alt="Third Place"
                src="../assets/svg/bronze.svg"
                class="trophy"
              />
            </td>
            <td v-else-if="index == 3">
              <img
                alt="Last Place"
                src="../assets/svg/hotdog.svg"
                class="trophy"
              />
            </td>
            <td v-for="entry in persons" v-bind:key="entry.id">{{ entry }}</td>
          </tr>
        </tbody>
      </table>

      <table>
        <thead>
          <tr>
            <th colspan="2" v-if="currentYearDataToDisplay">2018/19 Awards</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="details in year2018DataToDisplay.details"
            v-bind:key="details.id"
          >
            <td
              v-for="entrydetails in details"
              v-bind:key="entrydetails.id"
              class="season-details"
            >
              {{ entrydetails }}
            </td>
          </tr>
        </tbody>
      </table>
    </section>
  </main>
</template>

<script>
export default {
  name: "sheetRequest",
  data() {
    return {
      endpoint: "https://sheets.googleapis.com/v4/spreadsheets/",
      spreadsheet: process.env.VUE_APP_SPREADSHEETID,
      apiKey: process.env.VUE_APP_GOOGLE,
      currentYearSelection: "/values/currentapisheet!A1:B10?",
      yearSelection2022: "/values/2022apisheet!A1:B10?",
      yearSelection2021: "/values/2021apisheet!A1:B10?",
      yearSelection2020: "/values/2020apisheet!A1:B10?",
      yearSelection2019: "/values/2019apisheet!A1:B10?",
      yearSelection2018: "/values/2018apisheet!A1:B10?",
      inplaySelection: "/values/inplay!A1:C60?",
      currentSheetData: "",
      sheetData2018: "",
      sheetData2019: "",
      sheetData2020: "",
      sheetData2021: "",
      sheetData2022: "",
      inplayData: "",
      numberOfPeople: 4,
      numberOfYears: 2,
      numberOfHeadings: 2,
      numberOfHeadingsAndPeople: 6,
      currentYearDataToDisplay: "",
      year2018DataToDisplay: "",
      year2019DataToDisplay: "",
      year2020DataToDisplay: "",
      year2021DataToDisplay: "",
      year2022DataToDisplay: "",
      inplayDataToDisplay: "",
    };
  },
  created() {
    this.initCurrentYear();
    this.init2018();
    this.init2019();
    this.init2020();
    this.init2021();
    this.init2022();
    this.initInplay();
  },
  mounted() {},
  methods: {
    // Fetch data from spreadsheet, parse JSON and ca
    async fetchData(yearSelect, sheetSelection) {
      const response = await fetch(
        `${this.endpoint}${this.spreadsheet}${yearSelect}key=${this.apiKey}`
      ).then((response) => response.json());
      this.saveDataToSelection(response, sheetSelection);
    },

    // Check to see which selection property to save to
    saveDataToSelection(dataToSave, sheetSelection) {
      if (sheetSelection == "currentSheetData") {
        this.currentSheetData = dataToSave;
      } else if (sheetSelection == "sheetData2022") {
        this.sheetData2021 = dataToSave;
      } else if (sheetSelection == "sheetData2021") {
        this.sheetData2021 = dataToSave;
      } else if (sheetSelection == "sheetData2020") {
        this.sheetData2020 = dataToSave;
      } else if (sheetSelection == "sheetData2019") {
        this.sheetData2019 = dataToSave;
      } else if (sheetSelection == "sheetData2018") {
        this.sheetData2018 = dataToSave;
      } else {
        this.inplayData = dataToSave;
      }
    },

    createResultsTable(specificYear) {
      let tableData;
      if (specificYear == "currentSheetData") {
        tableData = this.currentSheetData.values;
      } else if (specificYear == "sheetData2022") {
        tableData = this.sheetData2021.values;
      } else if (specificYear == "sheetData2021") {
        tableData = this.sheetData2021.values;
      } else if (specificYear == "sheetData2020") {
        tableData = this.sheetData2020.values;
      } else if (specificYear == "sheetData2019") {
        tableData = this.sheetData2019.values;
      } else {
        tableData = this.sheetData2018.values;
      }

      const tableObj = {};

      const dataToTableObject = (tableData) => {
        tableObj.yearheading = tableData[0];
        tableObj.tableheading = tableData[1];
        tableObj.names = tableData.slice(
          this.numberOfHeadings,
          this.numberOfHeadingsAndPeople
        );
        tableObj.details = tableData.slice(this.numberOfHeadingsAndPeople);

        if (specificYear == "currentSheetData") {
          this.currentYearDataToDisplay = tableObj;
        } else if (specificYear == "sheetData2022") {
          this.year2021DataToDisplay = tableObj;
        } else if (specificYear == "sheetData2021") {
          this.year2021DataToDisplay = tableObj;
        } else if (specificYear == "sheetData2020") {
          this.year2020DataToDisplay = tableObj;
        } else if (specificYear == "sheetData2019") {
          this.year2019DataToDisplay = tableObj;
        } else {
          this.year2018DataToDisplay = tableObj;
        }
      };

      dataToTableObject(tableData);
    },

    sortByWinAmountAndAddDollarSign(specificYear) {
      if (specificYear == "currentSheetData") {
        this.currentYearDataToDisplay.names.sort(compare);
        this.currentYearDataToDisplay.names.forEach(addDollarSign);
      } else if (specificYear == "sheetData2021") {
        this.year2021DataToDisplay.names.sort(compare);
        this.year2021DataToDisplay.names.forEach(addDollarSign);
      } else if (specificYear == "sheetData2021") {
        this.year2021DataToDisplay.names.sort(compare);
        this.year2021DataToDisplay.names.forEach(addDollarSign);
      } else if (specificYear == "sheetData2020") {
        this.year2020DataToDisplay.names.sort(compare);
        this.year2020DataToDisplay.names.forEach(addDollarSign);
      } else if (specificYear == "sheetData2019") {
        this.year2019DataToDisplay.names.sort(compare);
        this.year2019DataToDisplay.names.forEach(addDollarSign);
      } else {
        this.year2018DataToDisplay.names.sort(compare);
        this.year2018DataToDisplay.names.forEach(addDollarSign);
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

      function addDollarSign(data) {
        data[1] = `£${data[1]}`;
      }
    },

    createInplayTable() {
      const tableObj = {};
      tableObj.tableheading = this.inplayData.values[0];
      tableObj.details = this.inplayData.values.slice(1);
      this.inplayDataToDisplay = tableObj;
    },

    // Init functions
    async initCurrentYear() {
      await this.fetchData(this.currentYearSelection, "currentSheetData");
      this.createResultsTable("currentSheetData");
      this.sortByWinAmountAndAddDollarSign("currentSheetData");
    },
    async init2022() {
      await this.fetchData(this.yearSelection2022, "sheetData2022");
      this.createResultsTable("sheetData2022");
      this.sortByWinAmountAndAddDollarSign("sheetData2022");
    },
    async init2021() {
      await this.fetchData(this.yearSelection2021, "sheetData2021");
      this.createResultsTable("sheetData2021");
      this.sortByWinAmountAndAddDollarSign("sheetData2021");
    },
    async init2020() {
      await this.fetchData(this.yearSelection2020, "sheetData2020");
      this.createResultsTable("sheetData2020");
      this.sortByWinAmountAndAddDollarSign("sheetData2020");
    },
    async init2019() {
      await this.fetchData(this.yearSelection2019, "sheetData2019");
      this.createResultsTable("sheetData2019");
      this.sortByWinAmountAndAddDollarSign("sheetData2019");
    },
    async init2018() {
      await this.fetchData(this.yearSelection2018, "sheetData2018");
      this.createResultsTable("sheetData2018");
      this.sortByWinAmountAndAddDollarSign("sheetData2018");
    },
    async initInplay() {
      await this.fetchData(this.inplaySelection, "inplay");
      this.createInplayTable();
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
