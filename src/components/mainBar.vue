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
        <a @click="scrollMeTo('app')"><button v-on:click="isHidden = !isHidden" class="btn fourth">
          OPTIONS
        </button></a>
      </div>
      <div class="leftBottom">
        <h1 class="w3-xxlarge w3-animate-top" style="color: #FBFAEF">
          Total border crossings in
          <span style="color: #F49278">{{ selectedArea }}</span> from
          <span style="color: #F49278">{{ selectedStartYear }}</span> to
          <span style="color: #F49278">{{ selectedEndYear }}</span>
        </h1>
      </div>
      <div class="w3-display-middle">
        <h1 class="w3-jumbo w3-animate-top w3-center" style="color: #F49278">
          {{ totalNumber }}
        </h1>
        <hr class="w3-border-grey" style="margin:auto;width:40%" />
        <h1 style="color: #FBFAEF" class="w3-large w3-center">
          A
          <span style="color: #F49278"> {{ percentChangeForDom }}%</span> change
          from border crossings between
          <span style="color: #F49278">{{ previousYearOrNot }}</span>
        </h1>
      </div>
    </div>
    <div
      v-if="!isHidden"
      class="w3-content w3-container w3-padding-64"
      style="background-color: orange"
    >
      <div class="justifyLeft w3-left" ref="app">
        <h1 style="color: white" class="w3-padding-12">START YEAR</h1>
        <div class="select">
          <select name="slct" v-model="selectedStartYear" id="slct">
            <option selected disabled>Choose an option</option>
            <option
              v-for="choice in dynamicYear"
              :key="choice"
              :value="choice"
              >{{ choice }}</option
            >
          </select>
        </div>
        <h1 style="color: white" class="w3-padding-12">START MONTH</h1>
        <div class="select">
          <select name="slct" v-model="selectedStartMonth" id="slct">
            <option selected disabled>Choose an option</option>
            <option
              v-for="choice in dataNonDynamicMonth"
              :key="choice"
              :value="choice"
              >{{ choice }}</option
            >
          </select>
        </div>
      </div>
      <div class="justifyRight w3-right">
        <h1 style="color: white" class="w3-padding-12">END YEAR</h1>
        <div class="select">
          <select name="slct" v-model="selectedEndYear" id="slct">
            <option selected disabled>Choose an option</option>
            <option
              v-for="choice in dynamicYear"
              :key="choice"
              :value="choice"
              >{{ choice }}</option
            >
          </select>
        </div>
        <h1 style="color: white" class="w3-padding-12">END MONTH</h1>
        <div class="select">
          <select name="slct" v-model="selectedEndMonth" id="slct">
            <option selected disabled>Choose an option</option>
            <option
              v-for="choice in dataNonDynamicMonth"
              :key="choice"
              :value="choice"
              >{{ choice }}</option
            >
          </select>
        </div>

        <!-- <div class="" :key="choice" v-for="choice in optionsForMeasure">
          <input
            class="w3-check"
            type="checkbox"
            v-bind:id="choice"
            v-bind:value="choice"
            v-model="selectedOptionsForMeasure"
          />
          <label class="">{{ choice }}</label>
        </div> -->
        <!-- <label class="container"
          >One
          <input type="radio" checked="checked" name="radio" />
          <span class="checkmark"></span>
        </label>
        <label class="container"
          >Two
          <input type="radio" name="radio" />
          <span class="checkmark"></span>
        </label>
        <label class="container"
          >Three
          <input type="radio" name="radio" />
          <span class="checkmark"></span>
        </label>
        <label class="container"
          >Four
          <input type="radio" name="radio" />
          <span class="checkmark"></span>
        </label> -->
      </div>
      <br><br>
      <div class="w3-center w3-padding-24">
        <h1 style="color: white" class="w3-padding-12">SELECTED AREA</h1>
        <div class="w3-center" :key="choice" v-for="choice in optionsForArea">
          <input
            class="w3-radio"
            type="radio"
            v-bind:id="choice"
            v-bind:value="choice"
            v-model="selectedArea"
          />
          <label class="">{{ choice }}</label>
        </div>
      </div>
      <div class="w3-left w3-padding-24">
        <h1 style="color: white" class="w3-padding-12">PORT</h1>
        <div class="" :key="choice" v-for="choice in optionsForPorts">
          <input
            class="w3-check"
            type="checkbox"
            checked=""
            v-bind:id="choice"
            v-bind:value="choice"
            v-model="selectedOptionsForPorts"
          />

          <label>{{ choice }}</label>
        </div>
      </div>
      <div class="w3-right w3-padding-24">
        <h1 style="color: white" class="w3-padding-12">MEASURE</h1>
        <div class="" :key="choice" v-for="choice in optionsForMeasure">
          <input
            class="w3-check"
            type="checkbox"
            checked=""
            v-bind:id="choice"
            v-bind:value="choice"
            v-model="selectedOptionsForMeasure"
          />

          <label>{{ choice }}</label>
        </div>
      </div>
      <!--
      <div class="w3-content w3-container w3-padding-64" id="about">
        <h3 class="w3-center">OPTIONS</h3>
        <h2>select start year</h2>
        <select v-model="selectedStartYear">
           inline object literal
          <option
            v-for="choice in dynamicYear"
            :key="choice"
            :value="choice"
            >{{ choice }}</option
          >
        </select>
        <h2>select start month</h2>
        <select v-model="selectedStartMonth">

          <option
            v-for="choice in dataNonDynamicMonth"
            :key="choice"
            :value="choice"
            >{{ choice }}</option
          >
        </select>
        <h2>select end year</h2>
        <select v-model="selectedEndYear">

          <option
            v-for="choice in dynamicYear"
            :key="choice"
            :value="choice"
            >{{ choice }}</option
          >
        </select>
        <h2>select end month</h2>
        <select v-model="selectedEndMonth">

          <option
            v-for="choice in dataNonDynamicMonth"
            :key="choice"
            :value="choice"
            >{{ choice }}</option
          >
        </select>
        <div class="w3-left w3-container w3-card-4">
          <h2>Area</h2>
          <div class="" :key="choice" v-for="choice in optionsForArea">
            <input
              class="w3-radio"
              type="radio"
              v-bind:id="choice"
              v-bind:value="choice"
              v-model="selectedArea"
            />
            <label class="">{{ choice }}</label>
          </div>
        </div>
        <div class="w3-left w3-container w3-card-4">
          <h2>Ports</h2>
          <div class="" :key="choice" v-for="choice in optionsForPorts">
            <input
              class="w3-check"
              type="checkbox"
              v-bind:id="choice"
              v-bind:value="choice"
              v-model="selectedOptionsForPorts"
            />
            <label class="">{{ choice }}</label>
          </div>
        </div>
        <div class="w3-right w3-container w3-card-4">
          <h2>Measure</h2>
          <div class="" :key="choice" v-for="choice in optionsForMeasure">
            <input
              class="w3-check"
              type="checkbox"
              v-bind:id="choice"
              v-bind:value="choice"
              v-model="selectedOptionsForMeasure"
            />
            <label class="">{{ choice }}</label>
          </div>
        </div>

        </div>
       -->
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
      dynamicYear: [
        // "2017",
        // "2018",
        // "2016",
        // "2015",
        // "2014",
        // "2013",
        // "2012",
        // "2011",
        // "2010",
        // "2009",
        // "2008",
        // "2007",
        // "2006",
        // "2005",
        // "2004",
        // "2003",
        // "2002",
        // "2001",
        // "2000",
        // "1999",
        // "1998",
        // "1997",
        // "1996"
      ],
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
      selectedStartYear: "2019",
      selectedStartMonth: "11",
      selectedEndYear: "2019",
      selectedEndMonth: "11",
      unpipedTime: [],
      previousYearOrNot: ""
    };
  },
  methods: {
    setGetCheckBoxes: function(data) {
      console.log("it has been run");
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
      console.log(filterOutput1);
      // //console.log(filterOutput1);
      var selectedOptionsForMeasure = this.selectedOptionsForMeasure;
      console.log(selectedOptionsForMeasure);
      var unpipedTime = filterOutput1.filter(function(item) {
        return selectedOptionsForMeasure.includes(item.measure);
      });
      this.unpipedTime = unpipedTime;
      //console.log("below is unpipedtime");
      //console.log(this.unpipedTime);
      this.getTimeDates(this.unpipedTime);
    },
    getTimeDates: function(unpipedTime) {
      console.log(unpipedTime);
      this.selectedEndSelect =
        this.selectedEndYear + "-" + this.selectedEndMonth + "-01T00:00:00.000";

      console.log(this.selectedEndSelect);
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
      console.log(numberFy);
      var totalValue = numberFy.reduce(reducer);
      //console.log(totalValue);
      totalValue.toLocaleString();
      //console.log("hosifdnsoifhsofih");
      //console.log(totalValue);
      var hello = this.formatNumber(totalValue);
      //console.log(hello);
      containerValue.length = 0;
      console.log(numberFy);

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
      var startYearWithoutSplice = Number(selectedStartYear) - 1;
      console.log(startYearWithoutSplice);
      console.log(selectedEndMonth);
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
      console.log(
        "https://data.transportation.gov/resource/keg4-3bc2.json?$limit=100000&$where=date between '" +
          //  startYearWithoutSplice +
          2019 +
          "-" +
          //  selectedStartMonth +
          11 +
          "-01' and '" +
          //  selectedEndYear +
          2019 +
          "-" +
          // selectedEndMonth +
          11 +
          "-01'"
      );
      console.log(
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
      );
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

          console.log(response.data);
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
      console.log(this.selectedEndYear);
      console.log(this.selectedEndMonth);
      console.log(this.selectedStartYear);
      console.log(this.selectedStartMonth);

      this.getDataAxios(
        this.selectedEndYear,
        this.selectedEndMonth,
        this.selectedStartYear,
        this.selectedStartMonth
      );
    },

    selectedOptionsForMeasure: function() {
      if (this.selectedArea == ["California"]) {
        console.log(this.selectedOptionsForMeasure);
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

        //console.log(blabla);
        var selectedEndYear = blabla.getFullYear();
        var data = [];
        for (var i = selectedEndYear; i >= 1996; i--) {
          // var dynamicData = this.dataNonDymanicYear
          data.push(i);
        }
        this.dynamicYear = data;
        console.log(caliData);
        var selectedEndMonth = blabla.getMonth();
        this.setGetCheckBoxes(caliData);
        this.getDataAxios(
          selectedEndYear,
          selectedEndMonth,
          this.selectedStartYear,
          this.selectedStartMonth
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
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
h1 {
  margin: 0 0 0.25em;
}

/* Reset Select */
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
/* Remove IE arrow */
select::-ms-expand {
  display: none;
}
/* Custom Select */
.select {
  position: relative;
  display: flex;
  width: 20em;
  height: 3em;
  line-height: 3;
  background: #2c3e50;
  overflow: hidden;
  border-radius: 0.25em;
}
select {
  flex: 1;
  padding: 0 0.5em;
  color: #fff;
  cursor: pointer;
}
/* Arrow */
.select::after {
  content: "\25BC";
  position: absolute;
  top: 0;
  right: 0;
  padding: 0 1em;
  background: #34495e;
  cursor: pointer;
  pointer-events: none;
  -webkit-transition: 0.25s all ease;
  -o-transition: 0.25s all ease;
  transition: 0.25s all ease;
}
/* Transition */
.select:hover::after {
  color: #f39c12;
}

/* .hello {
  margin: 0;
  position: relative;

  -ms-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
  background-color: white;
  opacity: 0.5;
  width: 100%;
  height: 200px;
  display: inline-block;
  text-align: center;

}
.mainText{
  margin: 0;
  position: absolute;
  top: 50%;
  left: 50%;
  -ms-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);

}
*/

.centered {
  position: fixed;
  top: 50%;
  left: 50%;

  /* bring your own prefixes */
  transform: translate(-50%, -50%);
  background-color: white;
  opacity: 0.5;
  display: flex;
  align-items: center;
  justify-content: ;
}
.centered-bottom {
  position: fixed;
  top: 75%;
  left: 50%;

  /* bring your own prefixes */
  transform: translate(-50%, -50%);
  background-color: white;
  opacity: 0.5;
  display: flex;
  align-items: left;
  justify-content: left;
}
.responsive-font-size {
  font-size: 3vw;
}
.optionsPart {
  width: 100%;
  background-color: pink;
  height: 100%;
  opacity: 0.6;
  transform: translate(-50%, -50%);
  position: fixed;
  top: 50%;
  left: 50%;

  /* bring your own prefixes */
}
.leftBottom {
  transform: translate(-25%, -25%);
  position: absolute;
  top: 75%;
  left: 25%;
}
.centerTop {
  transform: translate(-50%, -50%);
  position: absolute;
  top: 25%;
  left: 50%;
 }
/*
.container {
  display: block;
  position: relative;
  padding-left: 35px;
  margin-bottom: 12px;
  cursor: pointer;
  font-size: 22px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

/* Hide the browser's default radio button
.container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
}

/* Create a custom radio button
.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 25px;
  width: 25px;
  background-color: #2c3e50;
  border-radius: 50%;
}

/* On mouse-over, add a grey background color
.container:hover input ~ .checkmark {
  background-color: #2c3e50;
}

/* When the radio button is checked, add a blue background
.container input:checked ~ .checkmark {
  background-color: #7EC9D7;
}

/* Create the indicator (the dot/circle - hidden when not checked)
.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}
/* Show the indicator (dot/circle) when checked
.container input:checked ~ .checkmark:after {
  display: block;
}

/* Style the indicator (dot/circle)
.container .checkmark:after {
  top: 9px;
  left: 9px;
  width: 8px;
  height: 8px;
  border-radius: 50%;
  background: #7EC9D7;
} */
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
  border: 2px solid #7ec9d7;
  border-radius: 0.6em;
  color: #7ec9d7;
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
/* The container */
.container {
  display: block;
  position: relative;
  padding-left: 35px;
  margin-bottom: 12px;
  cursor: pointer;
  font-size: 22px;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

/* Hide the browser's default checkbox */
.container input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

/* Create a custom checkbox */
.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 25px;
  width: 25px;
  background-color: #eee;
}

/* On mouse-over, add a grey background color */
.container:hover input ~ .checkmark {
  background-color: #ccc;
}

/* When the checkbox is checked, add a blue background */
.container input:checked ~ .checkmark {
  background-color: #2196F3;
}

/* Create the checkmark/indicator (hidden when not checked) */
.checkmark:after {
  content: "";
  position: absolute;
  display: none;
}

/* Show the checkmark when checked */
.container input:checked ~ .checkmark:after {
  display: block;
}

/* Style the checkmark/indicator */
.container .checkmark:after {
  left: 9px;
  top: 5px;
  width: 5px;
  height: 10px;
  border: solid white;
  border-width: 0 3px 3px 0;
  -webkit-transform: rotate(45deg);
  -ms-transform: rotate(45deg);
  transform: rotate(45deg);
}
/* color hexes
#7EC9D7
#F49278
#FBFAEF
 */
</style>
<!--     name: 'mainBar',
    data () {
      return {
        defaultData : [],
        californiaData : [],
        selectedPorts : [],
        selectedMeasures : [],
        selected : 'California',
        options: ["California", "USA-Mexico"]
      }
    },
    methods: {
      getCheckBoxes: function (data) {
        var container = []
        for (var i = 0; i < data.length; i++) {
          container.push(data[i].port_name)
          var unique = [... new Set(container)]
          // // //console.log(unique);
        }
      }
    },
    watch: {
      selectedArea: function () {
        getCheckBoxes(this.defaultData)
      }
    },
    mounted: function () {
      axios.get("https://data.transportation.gov/resource/keg4-3bc2.json?border=US-Mexico%20Border").then(response => {
        this.defaultData = response.data;
        var defaultData = this.defaultData
        var caliData  = defaultData.filter(function(item) {
          return "CA".includes(item.state);
        });
        this.californiaData = caliData;
        getCheckBoxes(this.californiaData)

      })
    }

-->
