<template>
  <div class="container" v-cloak>
    <h1>{{ this.city }}, {{ this.country }}</h1>
        <div v-for="result in results">
          {{ result }}
        </div>
        <div v-if="!this.isHidden">
          <button v-on:click="convertDegrees()"> {{ this.text }}</button>
          {{ this.converted }}°{{ this.degrees }}
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
      results:   [],
      temp:      0,
      degrees:   'C',
      text:      'C to F',
      converted:  0,
      isHidden:   true,
      city:       '',
      country:    '',
      forecast:   ''
    };
  },

  methods : {
    getWeather() {
        axios.get('https://fcc-weather-api.glitch.me/api/current?lat='
         + this.latitude + '&lon=' + this.longitude)
        .then((response) => {
          this.results = response.data
          this.temp = response.data.main.temp
          this.converted = this.temp
          this.country = this.results.sys.country
          this.city = this.results.name
          this.forecast = this.results.weather.main
            + ": " + this.results.weather.description
        })
        this.isHidden = false
     },
    convertDegrees() {
      if (this.degrees == 'C') {
        this.text = 'F to C'
        this.converted = (this.temp * 9/5) + 32
        this.degrees = 'F'
      } else {
        this.text = 'C to F'
        this.converted = this.temp
        this.degrees = 'C'
      }
    }
  },
  created() {
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(position => {
        this.longitude = position.coords.longitude,
        this.latitude = position.coords.latitude,
        this.getWeather()
      })
    } else {
      this.error = "Geolocation is not supported by this browser.";
    }
  }
}
</script>
<style>
[v-cloak] {
    display: none;
  }
</style>
