<template>
  <div id="app" class="weather-app">
    Min: {{ min }}°C<br />
    Max: {{ max }}°C
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      min: 0,
      max: 255,
    };
  },
  methods: {
    getData(key) {
      fetch(
        `https://api.openweathermap.org/data/3.0/onecall?lat=33.44&lon=-94.04&exclude=hourly&appid=${key}`
      )
        .then((res) => res.json())
        .then((data) => {
          this.min = data?.daily?.temp?.min || 0;
          this.max = data?.daily?.temp?.max || 255;
        });
    },
  },
  mounted() {
    this.getData(process.env.VUE_APP_OPEN_WEATHER_API_KEY);
  },
};
</script>

<style scoped>
/* 
  "min": 271.72,
  "max": 282.21,
*/
:root {
  --min: v-bind(min);
  --max: v-bind(max);
  --min-rgb: rgb(var(--min), var(--min), var(--min));
  --max-rgb: rgb(var(--max), var(--max), var(--max));
}
.weather-app {
  background-image: linear-gradient(var(--min-rgb), var(--max-rgb));
}
</style>
