<template>
  <div class="hello">
    <b-form-group label="Inline radios (default)">
    <b-form-radio-group
      v-model="selected"
      :options="options"
      name="radio-inline"
    ></b-form-radio-group>
  </b-form-group>
  </div>
</template>

<script>
import axios from 'axios'
export default {

  name: 'mainBar',
  data() {
    return {
      defaultData : [],
      californiaData : [],
      selectedPorts : [],
      selectedMeasures : [],
      selectedArea : ["California"]
    }
  },
  methods: {

  },
  mounted: function () {
    axios.get("https://data.transportation.gov/resource/keg4-3bc2.json?border=US-Mexico%20Border").then(response => {
      this.defaultData = response.data;
      var defaultData = this.defaultData
      var lea  = defaultData.filter(function(item) {
        return "CA".includes(item.state);
      });
      this.californiaData = lea;


    })
  }
}
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
</style>
