<template>
  <div class="container">
    <h1>{{ this.results.name }} Weather</h1>
      <div class="location">
        <p>

        <button v-on:click="getWeather()">get weather</button>
        <div v-for="result in results">
          {{ result }}
        </div>

      </p>
      </div>
  </div>
</template>
<script>
import axios from 'axios'

export default {
  name: 'Weather',
  data() {
    return {
      longitude: 0,
      latitude:  0,
      error:     '',
      results:   []
    };
  },
  methods : {
    showPosition(position) {
      this.longitude = position.coords.longitude;
      this.latitude = position.coords.latitude;
    },
    getWeather() {
      axios.get('https://fcc-weather-api.glitch.me/api/current?lat='
       + this.latitude + '&lon=' + this.longitude)
      .then((response) => {
        this.results = response.data
      })
    },
    celsiusToF() {
      
    }
  },
  created() {
    if (navigator.geolocation) {
      navigator.geolocation.watchPosition(this.showPosition);
    } else {
      this.error = "Geolocation is not supported by this browser.";
    }
  }
}
</script>
<style>
</style>
