<template>
  <div class="container .text-primary" v-cloak >
    <div class="bkg" v-bind:style="{ 'background-image': 'url(' + iconUrl + ')' }">
    <h1>{{ this.city }}, {{ this.country }}</h1>
    <div id="content">
      <p>
        <h3>
        {{ this.currentTemp }}

      <toggle-button
        id="changed-font"
        @change="convertDegrees()"
        color="#82C7EB"
        :value="true"
        :labels="{checked: '°C', unchecked: '°F'}" />

        </h3>
      </p>
        <br>
      <img v-bind:src='this.icon'></img>
      {{ this.forecast }}
    <br>

    <button
    v-on:click="getLocation()" class="btn btn-primary col-2">Refresh</button>
  </div>
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
      temp:      0,
      currentTemp: 0,
      degrees:   '°C',
      city:      '',
      country:   '',
      forecast:  '',
      error:     '',
      results:   [],
      icon:      null
    };
  },
  ready: function() {
     this.getLocation();
   },
   computed: {
     iconUrl: function() {
       if (this.forecast.includes("Cloud")) {
         return require('./../assets/bright-cloud.jpg')
       } else if (this.forecast.includes("Rain")) {
         return require('./../assets/light-rain.jpg')
       } else if (this.forecast.includes("Clear")) {
         return require('./../assets/clear-sky.jpg')
       } else if (this.forecast.includes("Sun")) {
         return require('./../assets/sunny.jpg')
       }
     }
   },
  methods : {
    getLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(position => {
          this.longitude = position.coords.longitude,
          this.latitude = position.coords.latitude,
          console.log("this.longitude : " + this.longitude)
          console.log("this.latitude : " + this.latitude)

          this.getWeather()
        })
      } else {
        this.error = "Geolocation is not supported by this browser.";
      }
    },
    getWeather() {
        axios.get('https://fcc-weather-api.glitch.me/api/current?lat='
         + this.latitude + '&lon=' + this.longitude)
        .then((response) => {
          this.results = response.data
          this.temp = this.results.main.temp
          this.currentTemp = this.temp
          this.country = this.results.sys.country
          this.city = this.results.name
          this.forecast = this.results.weather[0].main + ": " + this.results.weather[0].description
          this.icon = this.results.weather[0].icon
        })
     },
    convertDegrees() {
      if (this.degrees == '°C') {
        this.currentTemp = (this.temp * 9/5) + 32
        this.degrees = '°F'
      } else {
        this.currentTemp = this.temp
        this.degrees = '°C'
      }
    }
  },
  created() {
    this.getLocation();
  }
}
</script>
<style>
@import "../../node_modules/bootstrap/dist/css/bootstrap.css";

[v-cloak] {
    display: none;
  }

body, html {
  width: 100%;
  height: 100%;
}

h1 {
  font-weight: normal;
}

h3 {
  font-size: 20px;
}

.container {
  text-align: center;
  font-family: arial;
}
.bkg {
  -webkit-border-radius: 50px;
  -moz-border-radius: 50px;
  background-size: cover;
  padding: 50px;
}
div.v-switch-core {
  height: 25px;
  font-size: 16px;
}
.vue-js-switch#changed-font {
  font-size: 17px;
}
</style>
