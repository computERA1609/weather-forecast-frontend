<!-- <template>
    <div class="weather-forecast">
      <h1>Weather Forecast Tracking</h1>
      <form @submit.prevent="addWeather">
        <input v-model="city" type="text" placeholder="City" required />
        <input v-model="temperature" type="number" placeholder="Temperature" required />
        <input v-model="condition" type="text" placeholder="Condition" required />
        <button type="submit">Add Weather</button>
      </form>
      <ul>
        <li v-for="(weather, index) in weatherList" :key="index">
          {{ weather.city }} - {{ weather.temperature }}°C - {{ weather.condition }}
        </li>
      </ul>
    </div>
  </template> -->

<template>
  <div class="weather-forecast">
    <h1 class="title">Weather Forecast Tracking</h1>
    <form @submit.prevent="addWeather" class="weather-form">
      <input
        v-model="city"
        type="text"
        placeholder="City"
        required
        class="input"
      />
      <input
        v-model="temperature"
        type="number"
        placeholder="Temperature"
        required
        class="input"
      />
      <input
        v-model="condition"
        type="text"
        placeholder="Condition"
        required
        class="input"
      />
      <button type="submit" class="button">Add Weather</button>
    </form>
    <ul class="weather-list">
      <li
        v-for="(weather, index) in weatherList"
        :key="index"
        class="weather-item"
      >
        {{ weather.city }} - {{ weather.temperature }}°C -
        {{ weather.condition }}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      city: "",
      temperature: null,
      condition: "",
      weatherList: [],
    };
  },
  mounted() {
    this.fetchWeatherData();
  },
  methods: {
    async addWeather() {
      try {
        await axios.post("/api/weather", {
          city: this.city,
          temperature: this.temperature,
          condition: this.condition,
        });
        this.fetchWeatherData();
        this.city = "";
        this.temperature = null;
        this.condition = "";
      } catch (error) {
        console.error("Error adding weather:", error);
      }
    },
    async fetchWeatherData() {
      /* try {
          const response = await axios.get('/api/weather');
          this.weatherList = response.data;
        } catch (error) {
          console.error('Error fetching weather data:', error);
        } */

      try {
        // Belirli bir şehir adına bağlı olarak hava durumu verilerini almak için API isteği yapılır.
        const response = await fetch(`/weather/${this.cityInput}`);

        // API yanıtı JSON formatında döner, bu yüzden response'ı JSON'a çeviriyoruz.
        this.weather = await response.json();
      } catch (error) {
        // İstek sırasında bir hata oluşursa, hata bilgisini konsola yazdırıyoruz.
        console.error("An error occurred:", error);
      }
    },
  },
};
</script>

<style scoped>
.weather-forecast {
  text-align: center;
  max-width: 600px;
  margin: 0 auto;
}

.title {
  font-size: 24px;
  margin-bottom: 20px;
}

.weather-form {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

.input {
  padding: 10px;
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.button {
  padding: 10px 20px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.weather-list {
  list-style: none;
  padding: 0;
}

.weather-item {
  margin-bottom: 10px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
</style>
