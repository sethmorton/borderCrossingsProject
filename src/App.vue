<template>
  <div id="app">
    <mainBar />
  </div>
</template>

<script>
import mainBar from "./components/mainBar.vue";

export default {
  name: "App",
  components: {
    mainBar
    
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

.bgimg,
.bgimg-2,
.bgimg-3 {
  background-attachment: fixed;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}


.bgimg {
  background-image: url("https://media.istockphoto.com/photos/mexico-border-picture-id504657149");
  min-height: 100%;
}

.bgimg-2 {
  background-image: url("/w3images/parallax2.jpg");
  min-height: 400px;
}


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

@media only screen and (max-device-width: 1600px) {
  .bgimg-1,
  .bgimg-2,
  .bgimg-3 {
    background-attachment: scroll;
    min-height: 400px;
  }
}
</style>
