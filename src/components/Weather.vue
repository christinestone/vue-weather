<template>
  <div class="container" v-cloak>
    <h1>{{ this.city }}, {{ this.country }}</h1>
    {{ this.forecast }}
    <br>
    {{ this.converted }}
    <toggle-button @change="convertDegrees()" color="#82C7EB" :value="true" :labels="{checked: 'C', unchecked: 'F'}" />

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
      temp:      0,
      converted: 0,
      degrees:   'C',
      city:      '',
      country:   '',
      forecast:  '',
      error:     '',
      results:   []
    };
  },

  methods : {
    getWeather() {
        axios.get('https://fcc-weather-api.glitch.me/api/current?lat='
         + this.latitude + '&lon=' + this.longitude)
        .then((response) => {

          this.results = response.data
          this.temp = this.results.main.temp
          this.converted = this.temp
          this.country = this.results.sys.country
          this.city = this.results.name
          this.forecast = this.results.weather[0].main + ": " + this.results.weather[0].description
        })
     },
    convertDegrees() {
      if (this.degrees == 'C') {
        this.converted = (this.temp * 9/5) + 32
        this.degrees = 'F'
      } else {
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
