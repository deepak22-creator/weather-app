<template>
  <div id="app">
    <div class="header container h-100 p-5">
      <h1>Weather App</h1>
      <input
        v-model="city"
        placeholder="Enter city"
        @keyup.enter="getWeather"
      />
      <button @click="getWeather">Get Weather</button>
    </div>
    <div v-if="weather" class="weather-card">
      <h2>Weather in {{ weather.name }}</h2>
      <p>Temperature: {{ weather.main.temp }}°C</p>
      <p>Condition: {{ weather.weather[0].description }}</p>
      <p>Humidity: {{ weather.main.humidity }}%</p>
      <p>Wind Speed: {{ weather.wind.speed }} m/s</p>
    </div>

    <div v-if="error" class="error">{{ error }}</div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "WeatherApp",
  data() {
    return {
      city: "",
      weather: null,
      error: null,
    };
  },
  methods: {
    async getWeather() {
      const apiKey = "7fd951f1122c632f509b616d3fa22db7";
      const apiUrl = `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=${apiKey}`;

      try {
        this.error = null;
        const response = await axios.get(apiUrl);
        this.weather = response.data;
      } catch (err) {
        this.weather = null;
        this.error = "Unable to fetch weather data. Please try again.";
      }
    },
  },
};
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  margin-top: 0px;
  background-image: url("./assets/background.jpg");
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  height: 100vh;
  border-radius: 5px;
}

input {
  padding: 10px;
  font-size: 16px;
  margin-right: 10px;
  border-radius: 5px;
}

button {
  padding: 10px 15px;
  border-radius: 20px;
  font-size: 16px;
  cursor: pointer;
  background-image: linear-gradient(to right, cyan, magenta);
}
.header {
  background-color: #212730;
  border-radius: 5px;
  color: #fff;
  text-align: center;
  font-family: "Times New Roman", Times, serif;
  margin-top: 0rem;
  padding-bottom: 20px;
  padding-top: 5px;
}

.weather-card {
  background-color: #e4b8b8;
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 16px;
  max-width: 300px;
  margin: 20px auto;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.weather-card h2 {
  margin-top: 0;
}

.weather-card p {
  margin: 8px 0;
}

.error {
  color: red;
  margin-top: 20px;
}
</style>
