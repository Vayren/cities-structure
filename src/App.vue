<template>
  <Chart :data="data"></Chart>
</template>

<script>
import Chart from './components/Chart.vue'

function getInfo(d) {
  let markup = '';

  if (d.type === 'region') {
    markup = `
      <ul class="card__info">
          <li>Population: ${d.population}</li>
          <li>Area: ${d.area}</li>
          <li>Population density: ${d.populationDensity}</li>
          <li>Cities: ${d.cities}</li>
          <li>Villages: ${d.villages}</li>
      </ul>
      <p class="card__children">District: ${d.district}</p>
    `;
  }

  if (d.type === 'district') {
    markup = `
      <ul class="card__info">
          <li>Population: ${d.population}</li>
          <li>Area: ${d.area}</li>
          <li>Population density: ${d.populationDensity}</li>
          <li>District center: ${d.districtСenter}</li>
      </ul>
      <p class="card__children">Community: ${d.community}</p>
    `;
  }

  if (d.type === "community") {
    markup = `
      <ul class="card__info">
          <li>Population: ${d.population}</li>
          <li>Area: ${d.area}</li>
          <li>Settlement: ${d.settlement}</li>
      </ul>
    `;
  }

  return markup;
}

function cardColor(type){
  switch(type) {
    case 'region':
      return {
        red: 136,
        green: 216,
        blue: 176,
        alpha: 1,
      }
    case 'community':
      return {
        red: 255,
        green: 204,
        blue: 92,
        alpha: 1,
      }
    case 'district':
      return {
        red: 255,
        green: 111,
        blue: 105,
        alpha: 1,
      } 
  }
}

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
        this.data = d.map(d => {
          const width = 500;
          const height = 300;
          const expanded = false;

          return {
            nodeId: d.id,
            parentNodeId :d.parentId,
            width,
            height,
            borderWidth: 1,
            borderRadius: 15,  
            borderColor: cardColor(d.type),
            backgroundColor: cardColor(d.type),
            template: this.createCard(d),
            connectorLineColor: {
              red: 192,
              green: 192,
              blue: 192,
              alpha: 1
            },
            connectorLineWidth: 5,
            dashArray: '',
            expanded: expanded 
          } 
        });
      });
  },
  methods: {
    createCard(d) {
      return `
      <div class="card">           
          <figure class="img-container">
            <img class="img" src="${d.imageUrl}">
          </figure> 
          <div class="details">
            <p class="card__title">${d.name}</p>
            ${getInfo(d)}
          </div>
      </div>`;
    },
  }
}
</script>

<style>
  *,
  *::before,
  *::after {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }

  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }

  .card {
    display: flex;
    padding: 20px;
  }

  .card__title {
    color: #fff;
    font-size: 30px;
    font-weight: bold;
  }

  .card__info {
    list-style: none;
    font-size: 20px;
    margin: 20px 0;
  }

  .card__info li:not(:last-child) {
    margin-bottom: 10px;
  }

  .card__children {
    font-size: 20px;
  }

  .details {
    list-style: none;
    margin: 0 20px;
  }

  .img-container {
    min-width: 100px;
    max-width: 100px;
    height: 100px;
    background-color: #fff;
    border-radius: 3px;
    margin-right: 20px;
    flex: 1;
  }

  .img {
    width: 100%;
    height: 100%;
    display: block;
  }
</style>
