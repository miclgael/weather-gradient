<template>
  <div
    class="weather-app"
    :style="`background-image:linear-gradient(to right,${gradientRGB})`"
  >
    <span class="temp temp--min"> {{ min }}°C </span>
    <span class="temp temp--max"> {{ max }}°C </span>
  </div>
</template>

<script>
import { mockData } from './mockData.js';
export default {
  name: 'App',
  data() {
    return {
      min: 0,
      max: 255,
    };
  },
  methods: {
    getData(key = null, mockData = {}) {
      if (key) {
        // This code will fetch the data, IF you have an API key.
        // For now we'll use mock data.
        fetch(
          `https://api.openweathermap.org/data/2.5/weather?lat=-37.840935&lon=144.946457&appid=${key}&units=metric`
        )
          .then((res) => res.json())
          .then((data) => {
            this.min = data.main.temp_min;
            this.max = data.main.temp_max;
          });
      } else {
        this.min = mockData.main.temp_min || 0;
        this.max = mockData.main.temp_max || 40;
      }
    },
  },
  computed: {
    gradientRGB() {
      const min = Math.round(this.min);
      const max = Math.round(this.max);
      return `rgb(${min},125,125),rgb(255,${max},125)`;
    },
  },
  mounted() {
    this.getData(null, mockData);
  },
};
</script>

<style>
html,
body {
  padding: 0;
  margin: 0;
  width: 100vw;
  height: 100vh;
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto,
    Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  font-size: larger;
}
.weather-app {
  position: relative;
  color: white;
  text-align: center;
  width: 100vw;
  height: 100vh;
}
.temp {
  display: block;
  position: fixed;
  bottom: 1rem;
  border: 2px solid white;
  padding: 1rem;
}
.temp--min {
  left: 1rem;
}
.temp--max {
  right: 1rem;
}
</style>
