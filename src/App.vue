<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <canvas id="mycanvas" width="1650" height="2400"></canvas> -->
    <MakeupPaint
      v-for="(d, idx) in data.Boxs"
      :key="idx"
      :data="d"
      msg="Welcome to Your Vue.js App"
    />
  </div>
</template>

<script>
import MakeupPaint from "./components/MakeupPaint.vue";

export default {
  name: "App",
  data() {
    return {
      data: [],
    };
  },
  components: {
    MakeupPaint,
  },
  created() {
    fetch("http://172.17.3.180:9999")
      .then(function (res) {
        return res.json();
      })
      .then((res) => {
        this.data = res;
      });
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
#mycanvas {
  border: 1px solid rgb(199, 198, 198);
}
</style>
