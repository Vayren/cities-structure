<template>
  <Chart :data="data"></Chart>
</template>

<script>
import Chart from './components/Chart.vue'

export default {
  name: 'App',
  data() {
    return {
      data: null
    };
  },
  components: {
    Chart
  },
  created() {
    fetch(
      "https://secure-basin-85712.herokuapp.com/api/cities"
    )
      .then(d => d.json())
      .then(d => {
        console.log("fetched data");
        this.data = d;
      });
  }
}
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
</style>
