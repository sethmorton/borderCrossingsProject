<template>
  <div id="app">
    <mainBar />
  </div>
</template>

<script>
import mainBar from "./components/mainBar.vue";
// import example from './components/example.vue'
export default {
  name: "App",
  components: {
    mainBar
    //example
  }
};
</script>

<style>
body,
h1,
h2,
h3,
h4,
h5,
h6 {
  font-family: "Lato", sans-serif;
}
body,
html,
#app {
  height: 100%;
  color: #777;
  line-height: 1.8;
}

body,
h1 {
  font-family: "Raleway", sans-serif;
}
body,
html {
  height: 100%;
}
/* Create a Parallax Effect */
.bgimg,
.bgimg-2,
.bgimg-3 {
  background-attachment: fixed;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

/* First image (Logo. Full height) */
.bgimg {
  background-image: url("https://lh3.googleusercontent.com/proxy/D-xaZnmKpg2G39LlHhufiwmPs1jWPPPTQK_vzJvF1RGIUMjcH4RtEmP1ZlM-nEErjZH9isFMHg6t9A8Ecc77yu1s0szC2HQcaHSQjNev5gcPilnRrxvNdj7W1Bw");
  min-height: 100%;
}

/* Second image (Portfolio) */
.bgimg-2 {
  background-image: url("/w3images/parallax2.jpg");
  min-height: 400px;
}

/* Third image (Contact) */
.bgimg-3 {
  background-image: url("/w3images/parallax3.jpg");
  min-height: 400px;
}

.w3-wide {
  letter-spacing: 10px;
}
.w3-hover-opacity {
  cursor: pointer;
}

/* Turn off parallax scrolling for tablets and phones */
@media only screen and (max-device-width: 1600px) {
  .bgimg-1,
  .bgimg-2,
  .bgimg-3 {
    background-attachment: scroll;
    min-height: 400px;
  }
}
</style>
