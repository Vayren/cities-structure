<template>
  <div  ref="svgElementContainer" ></div>
</template>

<script>
  import  TreeChart from "d3-org-chart";

  export default {
  template : `<div ref="svgElementContainer"></div>`,
  name: "Chart",
  props: ["data"],
  data() {
    return {
      chartReference: null
    };
  },
  watch: {
    data(value) {
      this.renderChart(value);
    }
  },
  created() {
    
  },
  methods: {
    renderChart(data) {      
      if (!this.chartReference) {
        this.chartReference = new TreeChart();
      }

      this.chartReference
        .container(this.$refs.svgElementContainer)
        .data(data)
        .svgWidth(500)
        .initialZoom(0.4)
        .render();
    }
  }
};
</script>


