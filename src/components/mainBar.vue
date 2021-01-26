Skip to content Search or jump to… Pull requests Issues Marketplace Explore
@smphotography smphotography / borderCrossingsProject 0 00 Code Issues 0 Pull
requests 0 Actions Projects 0 Wiki Security Insights Settings
borderCrossingsProject/src/components/mainBar.vue Seth Morton Commit For Deploy
- Integration of Moment.js 9f337a6 7 days ago 894 lines (838 sloc) 27.2 KB

<template>
  <div class="expand">
    <!-- Set background size and image -->
    <div class="bgimg w3-display-container w3-animate-opacity">
      <div class="centerTop">
        <h1 class="w3-xxlarge w3-animate-top" style="color: brown;">
          Total border crossings in
          <span style="color: black;">{{ SelectedArea }}</span>
          from
          <span style="color: black;">{{ StartMonthWord }}{{ '\xa0' }}</span>

          <span style="color: black;">{{ StartYear }}</span>
          to
          <span style="color: black;">{{ EndMonthWord }}{{ '\xa0' }}</span>
          <span style="color: black;">{{ EndYear }}:</span>
        </h1>
      </div>
      <div class="leftBottom">
        <div>
          <!-- Stylized button for modal -->
          <b-button class="btn fourth" v-b-modal.modal-scrollable>
            Options
          </b-button>
          <!-- Modal and Modal title -->
          <b-modal
            id="modal-scrollable"
            size="sm"
            scrollable
            title="Options: Query Options"
            description="query options"
          >
            <!-- Start of Query Options -->
            <div class="w3-display-container grid-container">
              <div class="grid-item-startmonth">
                <div style="text-align: left;" class="select">
                  <select name="slct" v-model="StartMonth" id="slct">
                    <option selected disabled>Start Month</option>
                    <option v-for="x in NonDynMonth" :key="x" :value="x">
                      {{ x }}
                    </option>
                  </select>
                </div>

                <div
                  style="text-align: left; padding-top: 10px;"
                  class="select"
                >
                  <select name="slct" v-model="StartYear" id="slct">
                    <option selected disabled>Start Year</option>
                    <option v-for="x in DynYear" :key="x" :value="x">
                      {{ x }}
                    </option>
                  </select>
                </div>
              </div>
              <div class="grid-item-starttime">
                <h2 style="text-align: left;">Start Time</h2>
              </div>
              <div class="grid-item-endmonth">
                <div style="text-align: left;" class="select">
                  <select name="slct" v-model="EndMonth" id="slct">
                    <option selected disabled>End Month</option>
                    <option v-for="x in NonDynMonth" :key="x" :value="x">
                      {{ x }}
                    </option>
                  </select>
                </div>

                <div
                  style="text-align: left; padding-top: 10px;"
                  class="select"
                >
                  <select name="slct" v-model="EndYear" id="slct">
                    <option selected disabled>End Year</option>
                    <option v-for="x in DynYear" :key="x" :value="x">
                      {{ x }}
                    </option>
                  </select>
                </div>
              </div>
              <div class="grid-item-endtime">
                <h2 style="text-align: left padding-bottom: 10px;">End Time</h2>
              </div>
              <div class="ports-grid">
                <h2 style="text-align: left;">Ports</h2>
                <div class="" :key="x" v-for="x in OptionsPorts">
                  <label class="checkbox-label">
                    <input
                      class=""
                      type="checkbox"
                      checked=""
                      v-bind:id="x"
                      v-bind:value="x"
                      v-model="SelecPorts"
                    />

                    {{ x }}
                  </label>
                  <br />
                </div>
              </div>
              <div class="measures-grid">
                <h2 style="text-align: left;">Measure</h2>
                <div class="" :key="x" v-for="x in OptionsMeasure">
                  <label class="checkbox-label">
                    <input
                      class=""
                      type="checkbox"
                      checked=""
                      v-bind:id="x"
                      v-bind:value="x"
                      v-model="SelecMeasures"
                    />

                    {{ x }}
                  </label>
                  <br />
                </div>
              </div>
              <div class="selected-area-grid">
                <h2>Area</h2>
                <div class="" :key="x" v-for="x in OptionsArea">
                  <label class="checkbox-label">
                    <input
                      class=""
                      type="radio"
                      checked=""
                      v-bind:id="x"
                      v-bind:value="x"
                      v-model="SelectedArea"
                    />

                    {{ x }}
                  </label>
                  <br />
                </div>
              </div>
            </div>
          </b-modal>
        </div>
      </div>
      <!-- Another container -->
      <div class="w3-display-middle">
        <h1 class="w3-jumbo w3-animate-top w3-center" style="color: black;">
          {{ Sum }}
        </h1>
        <hr class="w3-border-grey" style="margin: auto; width: 40%;" />
        <h1 style="color: brown; font-size: 1.5rem;" class="w3-center">
          A
          <span style="color: black;">{{ PercDiff }}%</span>
          <span style="color: black;">{{ InOrDe }}</span>
          from border crossings between
          <span ref="app" style="color: black;">
            {{
              StartMonthWord +
              ' ' +
              (StartYear - 1) +
              ' to ' +
              EndMonthWord +
              ' ' +
              (EndYear - 1)
            }}
          </span>
        </h1>
        <h5 class="w3-center" style="color: black; font-size: 0.7rem;">
          Data from BTS
        </h5>
      </div>
    </div>
  </div>
</template>

<script>
// import axios
import axios from 'axios'
import moment from 'moment'
export default {
  data: function () {
    return {
      EndYear: '',
      EndMonth: '',
      StartYear: '',
      StartMonth: '',
      SelecPorts: [],
      SelecMeasures: [],
      OptionsPorts: [],
      OptionsMeasure: [],
      CaliData: true,
      SelectedArea: 'California',
      OptionsArea: ['California', 'Entire US-Mexico Border'],
      DynYear: [],
      NonDynMonth: [
        '1',
        '2',
        '3',
        '4',
        '5',
        '6',
        '7',
        '8',
        '9',
        '10',
        '11',
        '12',
      ],
      Sum: '',
      PercDiff: '',
      InOrDe: '',
      hasBeenTouched: false,
      StartMonthWord: '',
      EndMonthWord: '',
      CheckCali: '',
    }
  },
  methods: {
    returnTimeQueriedData: function () {
      if (
        new Date(this.StartYear, this.StartMonth, 1) >
        new Date(this.EndYear, this.EndMonth, 1)
      ) {
        alert('INVALID DATE')
      }
            var StartYearForQuery = this.StartYear - 1
      if (this.CaliData) {
        this.CheckCali = '&state=California'
      } else {
        this.CheckCali = ''
      }
    const str =  `
    https://data.transportation.gov/resource/keg4-3bc2.json?$limit=100000&$where=date between '${StartYearForQuery}-${this.StartMonth}-01T00:00:00.000' and '${this.EndYear}-${this.EndMonth}-01T00:00:00.000'&border=US-Mexico Border${this.CheckCali}
    `
    console.log(str)
      axios
        .get(str)
        .then((response) => {
          var Data = response.data;
          console.log(Data)
          var Ports = Data.map((x) => x.port_name);
          console.log(Ports)
          var UniquePorts = [...new Set(Ports)]
          var Measure = Data.map((x) => x.measure)
          var UniqueMeasure = [...new Set(Measure)]
          this.OptionsPorts = UniquePorts
          this.OptionsMeasure = UniqueMeasure
          this.getSum(Data)
        })
    },
    getSum: function (Data) {
      console.log(Data)
      var SelecMeasures = this.SelecMeasures
      var SelecPorts = this.SelecPorts
      var PipePort = Data.filter((x) => SelecPorts.includes(x.port_name));
      console.log(PipePort)
      var PipeMeasure = PipePort.filter((x) =>
        SelecMeasures.includes(x.measure),
      );
      console.log(PipeMeasure)
      var StringCreationEndDate =
        this.EndYear +
        '-' +
        ('0' + this.EndMonth).slice(-2) +
        '-01T00:00:00.000';
        console.log(StringCreationEndDate)
      var StringCreationStartDate =
        this.StartYear +
        '-' +
        ('0' + this.StartMonth).slice(-2) +
        '-01T00:00:00.000'
        console.log(StringCreationStartDate)
      var StringCreationEndDateMinusOne =
        this.EndYear -
        1 +
        '-' +
        ('0' + this.EndMonth).slice(-2) +
        '-01T00:00:00.000'
      var StringCreationStartDateMinusOne =
        this.StartYear -
        1 +
        '-' +
        ('0' + this.StartMonth).slice(-2) +
        '-01T00:00:00.000'

      var FullPipedData = PipeMeasure.filter(
        (x) => {
          console.log(x.date);

          return x.date >= StringCreationStartDate && x.date <= StringCreationEndDate
        }
      );
      console.log(FullPipedData)
      var FullPipedDataSubtracted = PipeMeasure.filter(
        (x) =>
          x.date >= StringCreationStartDateMinusOne &&
          x.date <= StringCreationEndDateMinusOne,
      )
      var DataVal = []
      for (let i = 0; i < FullPipedData.length; i++) {
        DataVal.push(FullPipedData[i].value)
      }
      var Sum = DataVal.map(Number).reduce((a, b) => a + b)
      var SubtractedDataVal = []
      for (let i = 0; i < FullPipedDataSubtracted.length; i++) {
        SubtractedDataVal.push(FullPipedDataSubtracted[i].value)
      }
      var SumForPercent = SubtractedDataVal.map(Number).reduce((a, b) => a + b)
      var PercDiff = Math.ceil(((Sum - SumForPercent) / Sum) * 100 * 100) / 100
      var InOrDe = Sum > SumForPercent ? 'increase' : 'decrease'
      this.Sum = Sum
      this.PercDiff = PercDiff
      this.InOrDe = InOrDe
      var StartMonth = this.StartMonth - 1
      var EndMonth = this.EndMonth - 1
      this.EndMonthWord = moment()
        .month(+EndMonth)
        .format('MMMM')
      this.StartMonthWord = moment()
        .month(+StartMonth)
        .format('MMMM')
    },
  },
  mounted: function () {
    axios
      .get('https://data.bts.gov/id/keg4-3bc2.json?border=US-Mexico%20Border')
      .then((res) => {
        const IniData = res.data
        const FilterMaxDate = new Date(
          Math.max.apply(
            null,
            IniData.map((x) => new Date(x.date)),
          ),
        )
        console.log(FilterMaxDate)
        var EndYear = moment(Date.parse(FilterMaxDate)).year()
        var EndMonth = moment(Date.parse(FilterMaxDate)).month() + 1
        var StartYear = EndYear - 1
        var StartMonth = EndMonth
        this.EndYear = EndYear
        this.EndMonth = EndMonth
        this.StartYear = StartYear
        this.StartMonth = StartMonth
        var Ports = IniData.map((x) => x.port_name);
        console.log(Ports)
        var UniquePorts = [...new Set(Ports)]
        var Measure = IniData.map((x) => x.measure)
        var UniqueMeasure = [...new Set(Measure)]
        var YearsArray = []
        for (let i = EndYear; i > 1996; i--) {
          YearsArray.push(i)
        }
        this.OptionsPorts = UniquePorts
        this.OptionsMeasure = UniqueMeasure
        this.SelecPorts = UniquePorts
        this.SelecMeasures = UniqueMeasure
        this.DynYear = YearsArray
        this.returnTimeQueriedData()
      })
  },
  watch: {
    SelecPorts: function () {
      if (this.SelecPorts.length != this.OptionsPorts.length) {
        this.hasBeenTouched = true
      }
      if (this.hasBeenTouched) {
        this.returnTimeQueriedData()
      }
    },
    SelecMeasures: function () {
      if (this.SelecMeasures.length != this.OptionsMeasure.length) {
        this.hasBeenTouched = true
      }
      if (this.hasBeenTouched) {
        this.returnTimeQueriedData()
      }
    },
    SelectedArea: function () {
      this.CaliData = !this.CaliData
      this.returnTimeQueriedData()
    },
    StartYear: function () {
      this.returnTimeQueriedData()
    },
    StartMonth: function () {
      this.returnTimeQueriedData()
    },
    EndYear: function () {
      this.returnTimeQueriedData()
    },
    EndMonth: function () {
      this.returnTimeQueriedData()
    },
  },
}
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
h1,
h5 {
  font-family: 'Raleway', sans-serif;
  color: gray;
}
body,
html {
  height: 100%;
}
.bgimg {
  background-image: url('../assets/tijuanariver.jpg');
  min-height: 100%;
  background-position: center;
  background-size: cover;
}
.bgimg1 {
  background-image: url('https://image.freepik.com/foto-gratis/textura-viejo-fondo-papel-anaranjado-primer-estructura-carton-denso_113767-2088.jpg');
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
  font-family: 'Montserrat', sans-serif;
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
label.checkbox-label input[type='checkbox'] {
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
© 2020 GitHub, Inc. Terms Privacy Security Status Help Contact GitHub Pricing
API Training Blog About
