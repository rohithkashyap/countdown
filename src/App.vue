<template>
  <div>
    <h3>Current time: </h3>
    <h2>{{ timestamp }}</h2>
    <hr>
    <h3>Target date:</h3>
    <input type="datetime-local" v-model="newTargetDate" />
    <hr>
    <h3>Time to go: </h3>
    <h2> {{ timeDiff }}</h2>
  </div>
</template>
<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      timestamp: "",
      newTargetDate: this.toIsoString(new Date("2022/06/23 00:00")).slice(
        0,
        16
      ),
      timeDiff: 0
    };
  },
  created() {
    setInterval(this.getNow, 1000);
    setInterval(this.calculateDiff, 500);
  },
  methods: {
    getNow: function () {
      const dt = new Date();
      
      this.timestamp = `${
    (dt.getMonth()+1).toString().padStart(2, '0')}/${
    dt.getDate().toString().padStart(2, '0')}/${
    dt.getFullYear().toString().padStart(4, '0')} ${
    dt.getHours().toString().padStart(2, '0')}:${
    dt.getMinutes().toString().padStart(2, '0')}:${
    dt.getSeconds().toString().padStart(2, '0')}`;      
    },
    async calculateDiff() {
      let end = new Date(this.newTargetDate);
      let diff = end - new Date();
      this.timeDiff = this.dhm(diff);
    },
    toIsoString(date) {
      let tzo = -date.getTimezoneOffset(),
        dif = tzo >= 0 ? "+" : "-",
        pad = function (num) {
          let norm = Math.floor(Math.abs(num));
          return (norm < 10 ? "0" : "") + norm;
        };

      return (
        date.getFullYear() +
        "-" +
        pad(date.getMonth() + 1) +
        "-" +
        pad(date.getDate()) +
        "T" +
        pad(date.getHours()) +
        ":" +
        pad(date.getMinutes()) +
        ":" +
        pad(date.getSeconds()) +
        dif +
        pad(tzo / 60) +
        ":" +
        pad(tzo % 60)
      );
    },
    dhm(ms) {
      let days = Math.floor(ms / (24 * 60 * 60 * 1000));
      let daysms = ms % (24 * 60 * 60 * 1000);
      let hours = Math.floor(daysms / (60 * 60 * 1000));
      let hoursms = ms % (60 * 60 * 1000);
      let minutes = Math.floor(hoursms / (60 * 1000));
      let minutesms = ms % (60 * 1000);
      let sec = Math.floor(minutesms / 1000);
      return days + " days" + ', ' + hours + " hours " + minutes + " minutes " + sec + " seconds";
    },
  },
  computed: {

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
input[type="datetime-local"] {
  transform: scale(1.5);
}
</style>
