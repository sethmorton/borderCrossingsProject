Skip to content
Search or jump to…

Pull requests
Issues
Marketplace
Explore

@smphotography
smphotography
/
borderCrossingsProject
0
00
 Code Issues 0 Pull requests 0 Actions Projects 0 Wiki Security Insights Settings
borderCrossingsProject/src/components/mainBar.vue
 Seth Morton Commit For Deploy - Integration of Moment.js
9f337a6 7 days ago
894 lines (838 sloc)  27.2 KB

<template>
  <div class="expand">
    <!-- Set background size and image -->
    <div class="bgimg w3-display-container w3-animate-opacity ">
      <div class="centerTop">
        <h1 class="w3-xxlarge w3-animate-top" style="color: brown ">
          Total border crossings in
          <span style="color: black">{{ selectedArea }}</span> from
          <span style="color: black">{{ startYearMonth }} </span>

          <span style="color: black">{{ selectedStartYear }}</span> to
          <span style="color: black">{{ endYearMonth }} </span>
          <span style="color: black">{{ selectedEndYear }}:</span>
        </h1>
      </div>
      <div class="leftBottom">
        <div>
          <!-- Stylized button for modal -->
          <b-button class="btn fourth" v-b-modal.modal-scrollable
            >Options</b-button
          >
          <!-- Modal and Modal title -->
          <b-modal
            id="modal-scrollable"
            size="sm"
            scrollable
            title="Options: Query Options"
            description="query options"
          >
            <!-- Start of Query Options -->
            <div class="w3-display-container grid-container ">
              <div class="grid-item-startmonth">
                <div style="text-align: left" class="select">
                  <select name="slct" v-model="selectedStartMonth" id="slct">
                    <option selected disabled>Start Month</option>
                    <option
                      v-for="choice in dataNonDynamicMonth"
                      :key="choice"
                      :value="choice"
                      >{{ choice }}</option
                    >
                  </select>
                </div>

                <div style="text-align: left; padding-top: 10px" class="select">
                  <select name="slct" v-model="selectedStartYear" id="slct">
                    <option selected disabled>Start Year</option>
                    <option
                      v-for="choice in dynamicYear"
                      :key="choice"
                      :value="choice"
                      >{{ choice }}</option
                    >
                  </select>
                </div>
              </div>
              <div class="grid-item-starttime">
                <h2 style="text-align: left; ">Start Time</h2>
              </div>
              <div class="grid-item-endmonth">
                <div style="text-align: left" class="select">
                  <select name="slct" v-model="selectedEndMonth" id="slct">
                    <option selected disabled>End Month</option>
                    <option
                      v-for="choice in dataNonDynamicMonth"
                      :key="choice"
                      :value="choice"
                      >{{ choice }}</option
                    >
                  </select>
                </div>

                <div style="text-align: left; padding-top: 10px" class="select">
                  <select name="slct" v-model="selectedEndYear" id="slct">
                    <option selected disabled>Ennd Year</option>
                    <option
                      v-for="choice in dynamicYear"
                      :key="choice"
                      :value="choice"
                      >{{ choice }}</option
                    >
                  </select>
                </div>
              </div>
              <div class="grid-item-endtime">
                <h2 style="text-align: left padding-bottom: 10px;">End Time</h2>
              </div>
              <div class="ports-grid">
                <h2 style="text-align: left">Ports</h2>
                <div class="" :key="choice" v-for="choice in optionsForPorts">
                  <label class="checkbox-label">
                    <input
                      class=""
                      type="checkbox"
                      checked=""
                      v-bind:id="choice"
                      v-bind:value="choice"
                      v-model="selectedOptionsForPorts"
                    />

                    {{ choice }}</label
                  ><br />
                </div>
              </div>
              <div class="measures-grid">
                <h2 style="text-align: left">Measure</h2>
                <div class="" :key="choice" v-for="choice in optionsForMeasure">
                  <label class="checkbox-label">
                    <input
                      class=""
                      type="checkbox"
                      checked=""
                      v-bind:id="choice"
                      v-bind:value="choice"
                      v-model="selectedOptionsForMeasure"
                    />

                    {{ choice }}</label
                  ><br />
                </div>
              </div>
              <div class="selected-area-grid">
                <h2>Area</h2>
                <div class="" :key="choice" v-for="choice in optionsForArea">
                  <label class="checkbox-label">
                    <input
                      class=""
                      type="radio"
                      checked=""
                      v-bind:id="choice"
                      v-bind:value="choice"
                      v-model="selectedArea"
                    />

                    {{ choice }}</label
                  ><br />
                </div>
              </div>
            </div>
          </b-modal>
        </div>
      </div>
      <!-- Another container -->
      <div class="w3-display-middle">
        <h1 class="w3-jumbo w3-animate-top w3-center" style="color: black">
          {{ totalNumber }}
        </h1>
        <hr class="w3-border-grey" style="margin:auto;width:40%" />
        <h1 style="color: brown; font-size: 1.5rem;" class=" w3-center">
          A
          <span style="color: black"> {{ percentChangeForDom }}%</span>
          <span style="color: black"> {{ increaseOrDecrease }}</span>
          from border crossings between
          <span ref="app" style="color: black">{{ previousYearOrNot }}</span>
        </h1>
        <h5 class="w3-center " style="color: black; font-size: 0.7rem">Data from BTS</h5>
      </div>
    </div>
  </div>
</template>

<script>
// import axios
import axios from "axios";
import moment from "moment";
export default {
  // asign name for app.vue
  name: "mainBar",
  // every global variable
  data() {
    return {
      isHidden: true,
      defaultData: [],
      californiaData: [],
      selectedArea: "California",
      optionsForArea: ["California Border", "Entire USA-Mexico border"],
      selectedOptionsForPorts: [],
      optionsForPorts: [],
      selectedOptionsForMeasure: [],
      optionsForMeasure: [],
      optionsStartSelect: [],
      optionsEndSelect: [],
      selectedStartSelect: "2019-11-01T00:00:00.000",
      selectedEndSelect: "2019-12-01T00:00:00.000",
      totalNumber: "",
      dateForPageEnd: "",
      percentChangeForDom: "",
      dynamicYear: [],
      dataNonDynamicMonth: [
        "12",
        "11",
        "10",
        "09",
        "08",
        "07",
        "06",
        "05",
        "04",
        "03",
        "02",
        "01"
      ],
      selectedStartYear: "2018",
      selectedStartMonth: "12",
      selectedEndYear: "2019",
      selectedEndMonth: "12",
      unpipedTime: [],
      previousYearOrNot: "",
      startYearMonth: "",
      endYearMonth: "",
      increaseOrDecrease: "",
      hasBeenCreated: null
    };
  },
  // methods
  methods: {
    // set the query options
    setGetCheckBoxes: function(data) {
      // set empty arrays
      var containerPort = [];
      var containerMeasure = [];
      var containerTime = [];
      // for loop unique arrays
      for (let i = 0; i < data.length; i++) {
        containerPort.push(data[i].port_name);
        containerMeasure.push(data[i].measure);
        containerTime.push(data[i].date);
      }
      // uniqueArrays for checkboxes, selects, and radios
      var uniquePort = [...new Set(containerPort)];
      var uniqueMeasure = [...new Set(containerMeasure)];
      var uniqueTime = [...new Set(containerTime)];
      this.optionsStartSelect = uniqueTime;
      this.optionsEndSelect = uniqueTime;
      this.optionsForPorts = uniquePort;
      this.selectedOptionsForPorts = uniquePort;
      this.optionsForMeasure = uniqueMeasure;
      this.selectedOptionsForMeasure = uniqueMeasure;
    },
    // pipe the json for selected ports, selected measures
    pipeData: function(data) {
      console.log(data);
      var selectedOptionsForPorts = this.selectedOptionsForPorts;
      var filterOutput1 = data.filter(function(item) {
        return selectedOptionsForPorts.includes(item.port_name);
      });
      var selectedOptionsForMeasure = this.selectedOptionsForMeasure;
      var unpipedTime = filterOutput1.filter(function(item) {
        return selectedOptionsForMeasure.includes(item.measure);
      });
      this.unpipedTime = unpipedTime;
      // pass the filtered data into the time dates "getter"
      this.getTimeDates(this.unpipedTime);
    },
    // filter for selected time
    getTimeDates: function(unpipedTime) {

        var selectedEndMonth = ('0' + this.selectedEndMonth).slice(-2)
      console.log(selectedEndMonth);
      var selectedStartMonth = ('0' + this.selectedStartMonth).slice(-2)
      console.log(selectedStartMonth);

      this.selectedEndSelect =
        this.selectedEndYear + "-" + selectedEndMonth + "-01T00:00:00.000";
      var endDate = this.selectedEndSelect;
      console.log(endDate);
      this.selectedStartSelect =
        this.selectedStartYear +
        "-" +
        selectedStartMonth +
        "-01T00:00:00.000";
      var startDate = this.selectedStartSelect;
      this.pipeOriginalTimeData(startDate, endDate, unpipedTime);
    },
    // pass the filtered data and the time dates
    pipeOriginalTimeData(startDate, endDate, unpipedTime) {

      var pipedTime = unpipedTime.filter(function(obj) {
        return obj.date >= startDate && obj.date <= endDate;
      });
      console.log(pipedTime);

      // pass the filtered data into the percent function
      this.getNumbers(pipedTime, unpipedTime);
    },
    // get the total number
    getNumbers: function(pipedTime, unpipedTime) {
      console.log(pipedTime);
      // set empty array for sum
      var containerValue = [];
      for (var i = 0; i < pipedTime.length; i++) {
        containerValue.push(pipedTime[i].value);
      }
      // convert strings to numbers with map functions
      var numberFy = containerValue.map(Number);
      const reducer = (accumulator, currentValue) => accumulator + currentValue;
      // reduce (sum) the array
      var totalValue = numberFy.reduce(reducer);
      totalValue.toLocaleString();
      // format number method for insertion of commas
      var forComma = this.formatNumber(totalValue);
      // reset container value for next watch
      containerValue.length = 0;
      this.totalNumber = forComma;
      // get percent change
      this.percentChange(totalValue, pipedTime, unpipedTime);
    },
    // comma method
    formatNumber: function(num) {
      return num.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, "$1,");
    },
    // percentChange method to get percentchange for the years - 1 of the original time filter year
    percentChange: function(totalOldValue, pipedTime, filterBeforeTime) {
      console.log(filterBeforeTime);
      // set dates and subtract a year off of them
      // var endSelected = this.selectedEndSelect;
      // console.log(this.selectedEndSelect);
      //
      // var startSelected = this.selectedStartSelect;
      console.log(this.selectedEndSelect);
      var endDate = moment(this.selectedEndSelect)
      console.log(endDate);
      var startDate = moment(this.selectedStartSelect);
      var startDateMoment = moment(startDate).subtract(1, "years").format("YYYY-MM-DD")
      console.log(startDateMoment);
      var endDateMoment = moment(endDate).subtract(1, "years").format("YYYY-MM-DD")
      console.log(moment(startDateMoment, "YYYY/MM/DD").year());
     console.log(startDateMoment);
     console.log(endDateMoment);
      this.previousYearOrNot =
        this.startYearMonth +
        " " +
        moment(startDateMoment, "YYYY/MM/DD").year() +
        " to " +
        this.endYearMonth +
        " " +
        moment(endDateMoment, "YYYY/MM/DD").year();
      var startdatesteps = moment(startDateMoment, "YYYY/MM/DD").month() + 1;
      var startDateStepsIsoMonth = ('0' + startdatesteps).slice(-2)
      console.log(startDateStepsIsoMonth);
      var startDateStepsIsoYear = moment(startDateMoment, "YYYY/MM/DD").year();
      console.log(startDateStepsIsoYear);
      var enddatesteps = moment(endDateMoment, "YYYY/MM/DD").month() + 1;
      var endDateStepsIsoMonth = ('0' + enddatesteps).slice(-2)
      console.log(endDateStepsIsoMonth);
      var endDateStepsIsoYear = moment(endDateMoment, "YYYY/MM/DD").year();
      console.log(endDateStepsIsoYear);
      var startDateFinal =
      startDateStepsIsoYear +
        "-" +
        startDateStepsIsoMonth +
        "-01T00:00:00.000";
        console.log(startDateFinal);
      var endDateFinal =
        endDateStepsIsoYear +
        "-" +
        endDateStepsIsoMonth +
        "-01T00:00:00.000";
        console.log(endDateFinal);
      // filter the data ased on the newly created date strings
      var result = filterBeforeTime.filter(function(obj) {
        return obj.date >= startDateFinal && obj.date <= endDateFinal;
      });
      console.log(result);
      // same process as before, getting the total sum for the future math
      var containerValue = [];
      for (var i = 0; i < result.length; i++) {
        containerValue.push(result[i].value);
      }
      var numberFy1 = containerValue.map(Number);
      const reducer = (accumulator, currentValue) => accumulator + currentValue;
      var totalValue = numberFy1.reduce(reducer);
      // a series of variables for extracting commas and numberfy string for if statement
      var totalNumberOld = this.totalNumber;
      var replaceCommas = totalNumberOld.replace(/,/g, "");
      var replacedCommas = Number(replaceCommas);
      // is the value an increase since the previous years or not?
      if (replacedCommas > totalValue) {
        this.increaseOrDecrease = "increase";
      }
      if (replacedCommas < totalValue) {
        this.increaseOrDecrease = "decrease";
      }
      // this is where the math happens
      var percentChange = ((totalValue - totalOldValue) / totalValue) * 100;
      // rounding the very long number
      var percentChangeNew = function money_round(num) {
        //
        return Math.ceil(num * 100) / 100;
      };
      var forAbsolute = percentChangeNew(percentChange);
      // setting the global variable as the rounded percent change
      this.percentChangeForDom = Math.abs(forAbsolute);
    },
    getNewQueryOptions: function(data) {
      var containerPort = [];
      var containerMeasure = [];
      var containerTime = [];
      // for loop unique arrays
      for (let i = 0; i < data.length; i++) {
        containerPort.push(data[i].port_name);
        containerMeasure.push(data[i].measure);
        containerTime.push(data[i].date);
      }
      // uniqueArrays for checkboxes, selects, and radios
      var uniquePort = [...new Set(containerPort)];
      var uniqueMeasure = [...new Set(containerMeasure)];
      var uniqueTime = [...new Set(containerTime)];
      this.optionsStartSelect = uniqueTime;
      this.optionsEndSelect = uniqueTime;
      this.optionsForPorts = uniquePort;
      this.selectedOptionsForPorts = uniquePort;
      this.optionsForMeasure = uniqueMeasure;
      this.selectedOptionsForMeasure = uniqueMeasure;
    },
    minTwoDigits: function(n) {
      return (n < 10 ? "0" : "") + n;
    },
    getDataAxios: function(
      selectedEndYear,
      selectedEndMonth,
      selectedStartYear,
      selectedStartMonth
    ) {
      console.log(selectedEndYear);
      console.log(selectedEndMonth);
      console.log(selectedStartYear);
      console.log(selectedStartMonth);
      // this method is first, fetching data by dynamically inserting dates subtracted by one into the query to get enough json for the percent change function
      // subtracting start year by one for query
      var startYearSubtracted = Number(selectedStartYear) - 1;
      console.log(startYearSubtracted);
      // function for getting month name for dom
      var month_name = function(dt) {
        var mlist = [
          "Jan.",
          "Feb.",
          "Mar.",
          "Apr.",
          "May",
          "Jun.",
          "Jul.",
          "Aug.",
          "Sep.",
          "Oct.",
          "Nov.",
          "Dec."
        ];
        return mlist[dt.getMonth()];
      };

      // created dates for enddate bigger than startdate "filter"
      this.startYearMonth = month_name(
        new Date(selectedStartMonth + "/01/" + selectedStartYear)
      );
      this.endYearMonth = month_name(
        new Date(selectedEndMonth + "/01/" + selectedEndYear)
      );
      var startDate = new Date(selectedStartMonth + "/01/" + selectedStartYear);
      var endDate = new Date(selectedEndMonth + "/01/" + selectedEndYear);
      if (startDate > endDate) {
        alert(
          "Please ensure that the End Date is greater than or equal to the Start Date."
        );
      }
      // fetching data
      // limit is set to the max because i want to get ALL json from the time periods
      axios
        .get(
          "https://data.transportation.gov/resource/keg4-3bc2.json?$limit=100000&$where=date between '" +
            startYearSubtracted +
            "-" +
            selectedStartMonth +
            "-01' and '" +
            selectedEndYear +
            "-" +
            selectedEndMonth +
            "-01'"
        )
        .then(response => {
          // getting the response
          var reaction = response.data;
          // filtering the data down to only us-mexico
          var defaultData = reaction.filter(function(item) {
            return "US-Mexico Border".includes(item.border);
          });
          this.defaultData = defaultData;
          var defaultDataToCali = this.defaultData;
          // filtering data down to only california (this set of data will be used on default)
          var caliData = defaultDataToCali.filter(function(item) {
            return "CA".includes(item.state);
          });
          this.californiaData = caliData;
          // this reduces time as the method is only filtering what is neccesary
          if (this.selectedArea == ["California"]) {
            this.pipeData(caliData);
          }
          if (this.selectedArea == ["USA-Mexico"]) {
            this.pipeData(defaultData);
          }
        });
    }
  },
  created() {
    // this is on page load
    axios
      .get("https://data.transportation.gov/resource/keg4-3bc2.json?")
      .then(response => {
        var reactione = response.data;
        // get max date possible for query
        // var maxDate =
        //   Math.max.apply(
        //     null,
        //     reactione.map(function(e) {
        //       return e.date;
        //     })
        //   )
        var max = null;
        var min = null;
        for (var j = 0; j < reactione.length; j++) {
          var current = reactione[j];
          if (max === null || current.source > max.source) {
            max = current;
          }
          if (min === null || current.source < min.source) {
            min = current;
          }
        }
       //console.log(max.date);
        var day = moment(max.date);
        // moment(item.date,"YYYY/MM/DD").year()
        // var newDate = maxDate.setHours( maxDate.getHours() + 8 );
        //  //console.log(newDate);
        ////console.log(maxDate.getTime());
        // // var maxDateForSafari = maxDate.toUTCString();
        // var newDate = new Date(maxDate.getTime());
        ////console.log(newDate);
        var caliData = reactione.filter(function(item) {
          return "CA".includes(item.state);
        });
        // subtract the years
        var selectedEndYear = moment(day, "YYYY/MM/DD").year();
       console.log(selectedEndYear);
        var selectedEndMonth = moment(day, "YYYY/MM/DD").month() + 1;
       console.log(selectedEndMonth);
        var selectedStartMonth = moment(day, "YYYY/MM/DD").month() + 1;
        var selectedStartYear = moment(day, "YYYY/MM/DD").year() - 1;
        this.selectedEndYear = selectedEndYear
        this.selectedEndMonth =  ('0' + selectedEndMonth).slice(-2)
        console.log(selectedStartMonth);
        this.selectedStartMonth = ('0' + selectedStartMonth).slice(-2)
  this.selectedStartYear  = selectedStartYear
        var data = [];
        // loop until the dynamic year
        for (var i = selectedEndYear; i >= 1996; i--) {
          data.push(i);
        }
        // insert array of year
        this.dynamicYear = data;
        // call the axios method
        // set empty arrays
        var containerPort = [];
        var containerMeasure = [];
        var containerTime = [];
        // for loop unique arrays
        for (let i = 0; i < caliData.length; i++) {
          containerPort.push(caliData[i].port_name);
          containerMeasure.push(caliData[i].measure);
          containerTime.push(caliData[i].date);
        }
        // uniqueArrays for checkboxes, selects, and radios
        var uniquePort = [...new Set(containerPort)];
        var uniqueMeasure = [...new Set(containerMeasure)];
        var uniqueTime = [...new Set(containerTime)];
        this.optionsStartSelect = uniqueTime;
        this.optionsEndSelect = uniqueTime;
        this.optionsForPorts = uniquePort;
        this.selectedOptionsForPorts = uniquePort;
        this.optionsForMeasure = uniqueMeasure;
        this.selectedOptionsForMeasure = uniqueMeasure;
        this.hasBeenCreated = true;
        this.getDataAxios(
          selectedEndYear,
          selectedEndMonth,
          selectedStartYear,
          selectedStartMonth
        );
      });
  },
  watch: {
    // event listener for when any selected array changes
    // had problems because the listener would trigger on mounted so lots of code preventing trigger on mounted
    selectedArea: function() {
      if (this.selectedArea == "California") {
        this.getNewQueryOptions(this.californiaData);
      }
      if (this.selectedArea == "USA-Mexico") {
        this.getNewQueryOptions(this.defaultData);
      }
      this.getDataAxios(
        this.selectedEndYear,
        this.selectedEndMonth,
        this.selectedStartYear,
        this.selectedStartMonth
      );
    },
    selectedEndYear: function() {
      this.getDataAxios(
        this.selectedEndYear,
        this.selectedEndMonth,
        this.selectedStartYear,
        this.selectedStartMonth
      );
    },
    selectedEndMonth: function() {
      this.getDataAxios(
        this.selectedEndYear,
        this.selectedEndMonth,
        this.selectedStartYear,
        this.selectedStartMonth
      );
    },
    selectedStartYear: function() {
      this.getDataAxios(
        this.selectedEndYear,
        this.selectedEndMonth,
        this.selectedStartYear,
        this.selectedStartMonth
      );
    },
    selectedStartMonth: function() {
      this.getDataAxios(
        this.selectedEndYear,
        this.selectedEndMonth,
        this.selectedStartYear,
        this.selectedStartMonth
      );
    },
    selectedOptionsForMeasure: function() {
      var booboo = this.optionsForMeasure;
      var gaagaa = this.selectedOptionsForMeasure;
      if (gaagaa.length == booboo.length && this.hasBeenCreated) {
       console.log("the query options have not been touched");
      }
      if (gaagaa.length == booboo.length && this.hasBeenCreated == false) {
        if (this.selectedArea == "California") {
          this.pipeData(this.californiaData);
        }
        if (this.selectedArea == "USA-Mexico") {
          this.pipeData(this.defaultData);
        }
      }
      if (gaagaa.length != booboo.length) {
        this.hasBeenCreated = false;
        if (this.selectedArea == "California") {
          this.pipeData(this.californiaData);
        }
        if (this.selectedArea == "USA-Mexico") {
          this.pipeData(this.defaultData);
        }
      }
    },
    selectedOptionsForPorts: function() {
      var booboo = this.optionsForPorts;
      var gaagaa = this.selectedOptionsForPorts;
      if (gaagaa.length == booboo.length && this.hasBeenCreated) {
       console.log("the query options have not been touched");
      }
      if (gaagaa.length == booboo.length && this.hasBeenCreated == false) {
        if (this.selectedArea == "California") {
          this.pipeData(this.californiaData);
        }
        if (this.selectedArea == "USA-Mexico") {
          this.pipeData(this.defaultData);
        }
      }
      if (gaagaa.length != booboo.length) {
        this.hasBeenCreated = false;
        if (this.selectedArea == "California") {
          this.pipeData(this.californiaData);
        }
        if (this.selectedArea == "USA-Mexico") {
          this.pipeData(this.defaultData);
        }
      }
    }
  }
};
</script>
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
h1 {
  margin: 0 0 0.25em;
}
h2 {
  display: inline-block;
  white-space: nowrap;
}
body,
h1,h5 {
  font-family: "Raleway", sans-serif;
  color: gray;
}
body,
html {
  height: 100%;
}
.bgimg {
  background-image: url("../assets/tijuanariver.jpg");
  min-height: 100%;
  background-position: center;
  background-size: cover;
}
.bgimg1 {
  background-image: url("https://image.freepik.com/foto-gratis/textura-viejo-fondo-papel-anaranjado-primer-estructura-carton-denso_113767-2088.jpg");
  min-height: 100%;
  background-position: center;
  background-size: cover;
}
.leftBottom {
  transform: translate(-25%, -25%);
  position: absolute;
  top: 70%;
  left: 25%;
}
.centerTop {
  transform: translate(-50%, -50%);
  position: absolute;
  top: 20%;
  left: 50%;
}
.fourth {
  border-color: #f49278;
  color: #fff;
  background-image: linear-gradient(45deg, #f1c40f 50%, transparent 50%);
  background-position: 100%;
  background-size: 400%;
  -webkit-transition: background 300ms ease-in-out;
  transition: background 300ms ease-in-out;
}
.fourth:hover {
  background-position: 0;
}
.expand {
  width: 100%;
  height: 100%;
}
.btn {
  box-sizing: border-box;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;
  background-color: transparent;
  border: 2px solid white;
  border-radius: 0.6em;
  color: white;
  cursor: pointer;
  display: -webkit-box;
  display: flex;
  align-self: center;
  font-size: 1rem;
  font-weight: 400;
  line-height: 1;
  margin: 20px;
  padding: 1.2em 2.8em;
  text-decoration: none;
  text-align: center;
  text-transform: uppercase;
  font-family: "Montserrat", sans-serif;
  font-weight: 700;
}
.btn:hover,
.btn:focus {
  color: #fff;
  outline: 0;
}
.justifyLeft {
  display: flex;
  flex-direction: column;
  justify-content: left;
  align-items: left;
}
.justifyRight {
  display: flex;
  flex-direction: column;
  justify-content: right;
  align-items: right;
}
.grid-container {
  display: grid;
  grid-column-gap: 15px;
  grid-template-rows: repeat(4, 100px);
  grid-template-columns: repeat(3, 1fr);
  background-color: #fff;
  padding: 10px;
}
.grid-item-startmonth {
  grid-row-start: 2;
  grid-row-end: 3;
  grid-column-start: 2;
  grid-column-end: 3;
}
.grid-item-startyear {
  grid-row-start: 3;
  grid-row-end: 4;
  grid-column-start: 2;
  grid-column-end: 3;
  text-align: center;
  padding-top: 30px;
}
.grid-item-starttime {
  grid-row-start: 1;
  grid-row-end: 2;
  grid-column-start: 2;
  grid-column-end: 3;
  text-align: center;
  padding-top: 50px;
}
.grid-item-endmonth {
  grid-row-start: 3;
  grid-row-end: 4;
  grid-column-start: 2;
  grid-column-end: 3;
  padding-top: 50px;
  padding-bottom: 50px;
}
.grid-item-endyear {
  grid-row-start: 5;
  grid-row-end: 6;
  grid-column-start: 2;
  grid-column-end: 3;
  text-align: center;
  padding-top: 30px;
}
.grid-item-endtime {
  grid-row-start: 3;
  grid-row-end: 3;
  grid-column-start: 2;
  grid-column-end: 3;
  padding-bottom: 30px;
}
.ports-grid {
  grid-row-start: 7;
  grid-row-end: 10;
  grid-column-start: 2;
  grid-column-end: 3;
  padding-bottom: 30px;
}
.measures-grid {
  grid-row-start: 10;
  grid-row-end: 14;
  grid-column-start: 2;
  grid-column-end: 3;
}
label.checkbox-label input[type="checkbox"] {
  position: relative;
  vertical-align: middle;
  bottom: 1px;
  padding-top: 10px;
}
label {
  color: gray;
}
.selected-area-grid {
  grid-row-start: 4;
  grid-row-end: 6;
  grid-column-start: 2;
  grid-column-end: 3;
  padding-top: 60px;
  padding-bottom: 30px;
}
</style>
© 2020 GitHub, Inc.
Terms
Privacy
Security
Status
Help
Contact GitHub
Pricing
API
Training
Blog
About
