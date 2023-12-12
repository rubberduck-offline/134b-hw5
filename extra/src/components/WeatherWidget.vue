<template>
    <div>
      <h1>Current Weather</h1>
      <div id="weather-widget">
        <img :src="weatherIcon" alt="Weather Icon" />
        <p id="temperature">Temperature: {{ temperature }}°F</p>
        <p id="conditions">Conditions: {{ conditions }}</p>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        weatherIcon: '',
        temperature: '',
        conditions: '',
        errorMessage: '',
      };
    },
    mounted() {
      this.fetchWeatherData();
    },
    methods: {
      fetchWeatherData() {
        const apiUrl = 'https://api.weather.gov/points/32.8801,-117.2340';
  
        if ('fetch' in window) {
          fetch(apiUrl)
            .then((response) => response.json())
            .then((data) => {
              const forecastURL = data.properties.forecast;
              return fetch(forecastURL);
            })
            .then((response) => response.json())
            .then((data) => {
              this.weatherIcon = data.properties.periods[0].icon;
              this.temperature = data.properties.periods[0].temperature;
              this.conditions = data.properties.periods[0].shortForecast;
            })
            .catch((error) => {
              console.error('Error fetching weather data:', error);
              this.errorMessage = 'Current Weather Conditions Unavailable';
            });
        } else {
          this.errorMessage = 'JavaScript is disabled. Weather widget not available.';
        }
      },
    },
  };
  </script>
  
  <style scoped>
  #weather-widget {
    font-family: Arial, sans-serif;
    text-align: center;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 8px;
    width: 300px;
    margin: 50px auto;
  }
  
  #weather-widget img {
    width: 80px;
    height: 80px;
    margin-bottom: 10px;
  }
  
  .unavailable {
    color: red;
    font-weight: bold;
  }
  </style>
  