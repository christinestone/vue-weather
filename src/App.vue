<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
import Weather from './components/Weather';

export default {
  name: 'App',
  components: {
    Weather
  }
};
</script>

<style>

@import "../node_modules/bootstrap/dist/css/bootstrap.css";
</style>
