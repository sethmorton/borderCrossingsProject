<template>
  <div class="expand">
    <div class="bgimg w3-display-container w3-animate-opacity ">
      <!-- <div class="w3-display-middle">
        <h1 class="w3-jumbo w3-animate-top">
          <span style="color: #F49278">{{ totalNumber }}</span> <span style="color: #FBFAEF">a</span>
          <span style="color: #F49278">{{ percentChangeForDom }}</span> <span style="color: #FBFAEF">% change since</span>
          <span style="color: #F49278">{{ previousYearOrNot }} </span>
        </h1>

         <hr class="w3-border-grey" style="margin:auto;width:40%" />
        <p class="w3-large w3-center w3-animate-left">
          {{ totalNumber }} a <code>{{ percentChangeForDom }}</code
          >% change since {{previousYearOrNot}}
        </p>
        <button v-on:click="isHidden = !isHidden">Options</button>
      </div> -->
      <div class="centerTop">
        <!--  -->
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
          <b-button class="btn fourth" v-b-modal.modal-scrollable
            >Options</b-button
          >

          <b-modal
            id="modal-scrollable"
            size="sm"
            scrollable
            title="Options: Query Options"
            description="query options"
          >
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
              <!-- <div class="grid-item-startyear">
                <div class="select">
                  <select name="slct" id="slct">
                    <option selected disabled>Choose an option</option>
                    <option value="1">Pure CSS</option>
                    <option value="2">No JS</option>
                    <option value="3">Nice!</option>
                  </select>
                </div>
              </div> -->
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
              <!-- <div class="grid-item-startyear">
                <div class="select">
                  <select name="slct" id="slct">
                    <option selected disabled>Choose an option</option>
                    <option value="1">Pure CSS</option>
                    <option value="2">No JS</option>
                    <option value="3">Nice!</option>
                  </select>
                </div>
              </div> -->
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
                <!-- <label class="checkbox-label"><input type="checkbox" value="115">-middle</label><br>
                <label class="checkbox-label"><input type="checkbox" value="115">-ldlafjaldfjsllsdfj</label><br>
                <label class="checkbox-label"><input type="checkbox" value="115">-middle</label><br>
                <label class="checkbox-label"><input type="checkbox" value="115">lsknslknsldfknsldfksfasegsgagssg</label> -->
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
      <div class="w3-display-middle">
        <h1 class="w3-jumbo w3-animate-top w3-center" style="color: black">
          {{ totalNumber }}
        </h1>
        <hr class="w3-border-grey" style="margin:auto;width:40%" />
        <h1 style="color: brown; font-size: 1.5rem;" class=" w3-center">
          A
          <span style="color: black"> {{ percentChangeForDom }}%</span> change
          from border crossings between
          <span ref="app" style="color: black">{{ previousYearOrNot }}</span>
        </h1>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
// import Datepicker from 'vuejs-datepicker';
export default {
  // components: {
  //   Datepicker
  // },
  name: "mainBar",

  data() {
    return {
      isHidden: true,
      defaultData: [],
      californiaData: [],
      selectedArea: "California",
      optionsForArea: ["California", "USA-Mexico"],
      selectedOptionsForPorts: [],
      optionsForPorts: [],
      selectedOptionsForMeasure: [],
      optionsForMeasure: [],
      optionsStartSelect: [],
      optionsEndSelect: [],
      selectedStartSelect: "2019-11-01T00:00:00.000",
      selectedEndSelect: "2019-12-01T00:00:00.000",
      totalNumber: "",
      dateForPageStart: "",
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
        "01"
      ],
      selectedStartYear: "2018",
      selectedStartMonth: "12",
      selectedEndYear: "2019",
      selectedEndMonth: "12",
      unpipedTime: [],
      previousYearOrNot: "",
      startYearMonth: "",
      endYearMonth: ""
    };
  },
  methods: {
    setGetCheckBoxes: function(data) {
      // console.log("it has been run");
      var containerPort = [];
      var containerMeasure = [];
      var containerTime = [];
      for (let i = 0; i < data.length; i++) {
        containerPort.push(data[i].port_name);
        containerMeasure.push(data[i].measure);
        containerTime.push(data[i].date);
      }
      var uniquePort = [...new Set(containerPort)];
      var uniqueMeasure = [...new Set(containerMeasure)];
      var uniqueTime = [...new Set(containerTime)];
      this.optionsStartSelect = uniqueTime;
      this.optionsEndSelect = uniqueTime;
      this.optionsForPorts = uniquePort;
      //console.log(uniquePort);
      //console.log("uniquePort");
      this.selectedOptionsForPorts = uniquePort;
      this.optionsForMeasure = uniqueMeasure;
      this.selectedOptionsForMeasure = uniqueMeasure;
    },
    pipeData: function(data) {
      //console.log(data);
      var selectedOptionsForPorts = this.selectedOptionsForPorts;
      var filterOutput1 = data.filter(function(item) {
        return selectedOptionsForPorts.includes(item.port_name);
      });
      // console.log(filterOutput1);
      // //console.log(filterOutput1);
      var selectedOptionsForMeasure = this.selectedOptionsForMeasure;
      // console.log(selectedOptionsForMeasure);
      var unpipedTime = filterOutput1.filter(function(item) {
        return selectedOptionsForMeasure.includes(item.measure);
      });
      this.unpipedTime = unpipedTime;
      //console.log("below is unpipedtime");
      //console.log(this.unpipedTime);
      this.getTimeDates(this.unpipedTime);
    },
    getTimeDates: function(unpipedTime) {
      // console.log(unpipedTime);
      this.selectedEndSelect =
        this.selectedEndYear + "-" + this.selectedEndMonth + "-01T00:00:00.000";

      // console.log(this.selectedEndSelect);
      //console.log("loooser");
      var endDate = this.selectedEndSelect;
      //console.log(endDate);
      this.selectedStartSelect =
        this.selectedStartYear +
        "-" +
        this.selectedStartMonth +
        "-01T00:00:00.000";

      //console.log(this.selectedStartSelect);
      var startDate = this.selectedStartSelect;
      //console.log(startDate);
      //console.log("below is unpiped time");
      //console.log(unpipedTime);
      this.pipeOriginalTimeData(startDate, endDate, unpipedTime);
    },
    pipeOriginalTimeData(startDate, endDate, unpipedTime) {
      var pipedTime = unpipedTime.filter(function(obj) {
        return obj.date >= startDate && obj.date <= endDate;
      });

      //console.log(pipedTime);
      this.getNumbers(pipedTime, unpipedTime);
    },
    getNumbers: function(pipedTime, unpipedTime) {
      //console.log(pipedTime);
      //console.log(unpipedTime);
      var containerValue = [];
      for (var i = 0; i < pipedTime.length; i++) {
        containerValue.push(pipedTime[i].value);
      }
      var numberFy = containerValue.map(Number);
      const reducer = (accumulator, currentValue) => accumulator + currentValue;
      // console.log(numberFy);
      var totalValue = numberFy.reduce(reducer);
      //console.log(totalValue);
      totalValue.toLocaleString();
      //console.log("hosifdnsoifhsofih");
      //console.log(totalValue);
      var hello = this.formatNumber(totalValue);
      //console.log(hello);
      containerValue.length = 0;
      // console.log(numberFy);

      this.totalNumber = hello;

      // //console.log(filterOutput2);

      this.percentChange(totalValue, pipedTime, unpipedTime);
    },
    formatNumber: function(num) {
      return num.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, "$1,");
    },
    percentChange: function(totalOldValue, pipedTime, filterBeforeTime) {
      //console.log(totalOldValue);
      //console.log(pipedTime);
      //console.log(filterBeforeTime);
      var endSelected = this.selectedEndSelect;
      //console.log("omg");
      //console.log(endSelected);
      //console.log("omg");
      var startSelected = this.selectedStartSelect;
      // //console.log(endSelected);
      // //console.log(startSelected);
      var endDate = new Date(endSelected);
      var dateForPageEnd = endDate.getFullYear();
      //console.log(dateForPageEnd);
      this.dateForPageEnd = dateForPageEnd;
      //console.log(endDate);
      //console.log("helloworld");

      endDate.setMonth(endDate.getMonth() - 12);
      //console.log(endDate);

      var startDate = new Date(startSelected);
      var dateForPageStart = startDate.getFullYear();
      //console.log(dateForPageStart);
      this.dateForPageStart = dateForPageStart;

      startDate.setMonth(startDate.getMonth() - 12);
      this.previousYearOrNot =
        startDate.getFullYear() + " to " + endDate.getFullYear();
      //console.log("blablabla");
      //console.log(startDate);
      //console.log("seperator");

      //console.log(filterBeforeTime);
      //console.log("seperator");
      var startDateStepsIsoMonth = startDate.getMonth() + 1;
      var startDateStepsIsoYear = startDate.getFullYear();
      var endDateStepsIsoMonth = endDate.getMonth() + 1;
      var endDateStepsIsoYear = endDate.getFullYear();
      var startDateFinal =
        startDateStepsIsoYear +
        "-" +
        startDateStepsIsoMonth +
        "-01T00:00:00.000";
      var endDateFinal =
        endDateStepsIsoYear + "-" + endDateStepsIsoMonth + "-01T00:00:00.000";
      //console.log(startDateFinal);
      //console.log(endDateFinal);
      //console.log("below is startDateStepsIsoYear");
      //console.log(startDateStepsIsoYear);
      //console.log("below is startDateStepsIso Month");
      //console.log(startDateStepsIsoMonth);
      //console.log("seperator");
      var result = filterBeforeTime.filter(function(obj) {
        return obj.date >= startDateFinal && obj.date <= endDateFinal;
      });

      //console.log(result);
      //console.log("seperator above is result value");
      //console.log(totalOldValue);
      //console.log("seperator");
      var containerValue = [];
      for (var i = 0; i < result.length; i++) {
        containerValue.push(result[i].value);
      }
      //console.log(containerValue);
      var numberFy1 = containerValue.map(Number);
      const reducer = (accumulator, currentValue) => accumulator + currentValue;

      var totalValue = numberFy1.reduce(reducer);
      //console.log(totalValue);

      var percentChange = ((totalValue - totalOldValue) / totalValue) * 100;

      //console.log(percentChange);
      var percentChangeNew = function money_round(num) {
        return Math.ceil(num * 100) / 100;
      };
      this.percentChangeForDom = percentChangeNew(percentChange);
      //console.log(this.percentChangeForDom);
      //console.log(this.selectedArea);
    },
    getDataAxios: function(
      selectedEndYear,
      selectedEndMonth,
      selectedStartYear,
      selectedStartMonth
    ) {
      // console.log(selectedStartMonth);

      var startYearWithoutSplice = Number(selectedStartYear) - 1;

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
      // // console.log(
      //    month_name(new Date(selectedStartMonth + "/01/" + selectedStartYear))
      //  );
      this.startYearMonth = month_name(
        new Date(selectedStartMonth + "/01/" + selectedStartYear)
      );
      // console.log(selectedEndMonth + "/01/" + selectedEndYear);
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
      // //console.log(
      //   "https://data.transportation.gov/resource/keg4-3bc2.json?$limit=100000&$where=date between '" +
      //     startYearWithoutSplice +
      //     "-" +
      //     selectedStartMonth +
      //     "-01' and '" +
      //     selectedEndYear +
      //     "-" +
      //     selectedEndMonth +
      //     "-01'"
      // );

      // // console.log(
      //    "https://data.transportation.gov/resource/keg4-3bc2.json?$limit=100000&$where=date between '" +
      //      startYearWithoutSplice +
      //      // 2019 +
      //      "-" +
      //      selectedStartMonth +
      //      // 11 +
      //      "-01' and '" +
      //      selectedEndYear +
      //      // 2019 +
      //      "-" +
      //      selectedEndMonth +
      //      // 11 +
      //      "-01'"
      //  );
      axios
        .get(
          "https://data.transportation.gov/resource/keg4-3bc2.json?$limit=100000&$where=date between '" +
            startYearWithoutSplice +
            // 2019 +
            "-" +
            selectedStartMonth +
            // 11 +
            "-01' and '" +
            selectedEndYear +
            // 2019 +
            "-" +
            selectedEndMonth +
            // 11 +
            "-01'"
          // https://data.transportation.gov/resource/keg4-3bc2.json?$where=date%20between%20%271996-01-01T00:00:00.000%27%20and%20%272019-01-01T00:00:00.000%27
        )
        .then(response => {
          var reaction = response.data;

          // console.log(response.data);
          var defaultData = reaction.filter(function(item) {
            return "US-Mexico Border".includes(item.border);
          });
          //console.log(defaultData);
          this.defaultData = defaultData;
          // var container = []
          // for (var i = 0; i < defaultData.length; i++) {
          //   container.push(defaultData[i].date);
          //
          // }
          // var uniqueContainer = [... new Set(container)]

          // // //console.lxog(this.defaultData);
          var defaultDataToCali = this.defaultData;
          var caliData = defaultDataToCali.filter(function(item) {
            return "CA".includes(item.state);
          });

          //console.log(this.californiaData);
          if (this.selectedArea == ["California"]) {
            this.pipeData(caliData);
          }
          if (this.selectedArea == ["USA-Mexico"]) {
            this.pipeData(defaultData);
          }
        });
    },
    scrollMeTo(refName) {
      var element = this.$refs[refName];
      var top = element.offsetTop;

      window.scrollTo(0, top);
    }
  },
  watch: {
    selectedArea: function() {
      //console.log(this.selectedArea);
      //console.log(this.defaultData);
      if (this.selectedArea == ["California"]) {
        //console.log("California");
        this.setGetCheckBoxes(this.californiaData);
        this.getDataAxios(
          this.selectedEndYear,
          this.selectedEndMonth,
          this.selectedStartYear,
          this.selectedStartMonth
        );
      }
      if (this.selectedArea == ["USA-Mexico"]) {
        //console.log("USA-MEXICO");
        this.setGetCheckBoxes(this.defaultData);
        this.getDataAxios(
          this.selectedEndYear,
          this.selectedEndMonth,
          this.selectedStartYear,
          this.selectedStartMonth
        );
      }
    },

    selectedEndYear: function() {
      //console.log("googoogaaggaaa");
      //console.log(this.selectedEndYear);
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
      ////console.log("googoogaaggaaa");
      //console.log(this.selectedEndYear);
      this.getDataAxios(
        this.selectedEndYear,
        this.selectedEndMonth,
        this.selectedStartYear,
        this.selectedStartMonth
      );
    },
    selectedStartMonth: function() {
      // console.log(this.selectedEndYear);
      // console.log(this.selectedEndMonth);
      // console.log(this.selectedStartYear);
      // console.log(this.selectedStartMonth);

      this.getDataAxios(
        this.selectedEndYear,
        this.selectedEndMonth,
        this.selectedStartYear,
        this.selectedStartMonth
      );
    },

    selectedOptionsForMeasure: function() {
      if (this.selectedArea == ["California"]) {
        // console.log(this.selectedOptionsForMeasure);
        this.pipeData(this.californiaData);
      }
      if (this.selectedArea == ["USA-Mexico"]) {
        this.pipeData(this.defaultData);
      }
    },
    selectedOptionsForPorts: function() {
      if (this.selectedArea == ["California"]) {
        this.pipeData(this.californiaData);
      }
      if (this.selectedArea == ["USA-Mexico"]) {
        this.pipeData(this.defaultData);
      }
    }
  },
  mounted() {
    axios
      .get("https://data.transportation.gov/resource/keg4-3bc2.json")
      .then(response => {
        var reactione = response.data;
        /* eslint-disable no-unused-vars */
        var blabla = new Date(
          Math.max.apply(
            null,
            reactione.map(function(e) {
              return new Date(e.date);
            })
          )
        );
        // console.log(blabla);
        var defaultData = reactione.filter(function(item) {
          return "US-Mexico Border".includes(item.border);
        });
        //console.log(defaultData);
        this.defaultData = defaultData;
        // var container = []
        // for (var i = 0; i < defaultData.length; i++) {
        //   container.push(defaultData[i].date);
        //
        // }
        // var uniqueContainer = [... new Set(container)]

        // // //console.lxog(this.defaultData);
        var defaultDataToCali = this.defaultData;
        var caliData = defaultDataToCali.filter(function(item) {
          return "CA".includes(item.state);
        });
        // //console.log(this.date);
        // //  // // //console.log(caliData);
        this.californiaData = caliData;

        // console.log(blabla);
        // console.log(blabla.getMonth() + 1);
        var selectedEndYear = blabla.getFullYear();
        var selectedEndMonth = blabla.getMonth() + 1;

        var selectedStartYear = blabla.getFullYear() - 1;
        var data = [];
        for (var i = selectedEndYear; i >= 1996; i--) {
          // var dynamicData = this.dataNonDymanicYear
          data.push(i);
        }
        this.dynamicYear = data;
        // console.log(caliData);

        this.setGetCheckBoxes(caliData);
        this.getDataAxios(
          selectedEndYear,
          selectedEndMonth,
          selectedStartYear,
          selectedEndMonth
        );
      });
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
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
h1 {
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
} /*
select {
  -webkit-appearance: none;
  -moz-appearance: none;
  -ms-appearance: none;
  appearance: none;
  outline: 0;
  box-shadow: none;
  border: 0 !important;
  background: #2c3e50;
  background-image: none;
}

select::-ms-expand {
  display: none;
}

.select {
  position: relative;
  display: flex;
  width: 20em;
  height: 3em;
  line-height: 3;
  background: #2c3e50;
  overflow: hidden;
  border-radius: .25em;
}
select {
  flex: 1;
  padding: 0 .5em;
  color: #fff;
  cursor: pointer;
}

.select::after {
  content: '\25BC';
  position: absolute;
  top: 0;
  right: 0;
  padding: 0 1em;
  background: #34495e;
  cursor: pointer;
  pointer-events: none;
  -webkit-transition: .25s all ease;
  -o-transition: .25s all ease;
  transition: .25s all ease;
}
\
.select:hover::after {
  color: #f39c12;
} */
/*
#7EC9D7
#F49278
#FBFAEF
  */
</style>
