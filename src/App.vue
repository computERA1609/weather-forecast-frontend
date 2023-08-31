<template>
  <div>
  <div id="app">
    <h1>Weather Forecast Tracking</h1>
    <input v-model="cityInput" placeholder="Enter city" />
    <button @click="getWeather">Get Weather</button>
    
    <Weather v-if="weather" :city="cityInput" />
   
  </div>

  <div>
    <table>
      <tr>
        <th>City</th>
        <th>Temperature</th>
        <th>Condition</th>
        <th>Date</th>
      </tr>
      <tr v-for="item in this.list" :key="item.id">
            <td>{{ item.city }}</td>
            <td>{{ item.temperature }} °C</td>
            <td>{{item.condition}}</td>
            <td>{{item.date}}</td>
          </tr>
    </table>
  </div>
  </div>
</template>

<script>
import Weather from './components/WeatherForecast.vue';

export default {
  name: 'App',
  components: {
    Weather,
  },
  props: {
    history: Array,
  },
  data() {
    return {
      cityInput: '',
      weather: null,
      list: [],
    };
  },
  methods: {
    async getWeather() {
      console.log("this.cityInput", this.cityInput)
      if (this.cityInput) {
        const options = {
          method: "GET",
          //headers:{'Content-Type': 'application/json'}, // <-- This is the important part!
          //body: JSON.stringify({city:this.cityInput})  // <-- Ditto here...
        }
        try {
          const response = await fetch(`http://localhost:3000/api/weather-search/${this.cityInput}`, options);
          let data = await response.json();
          console.log('yeni data', data);
          if (data.success) {
            this.getHistory()
          }

        } catch (error) {
          console.error('An error occurred:', error);
        }
      }
    },
    async getHistory() {
      console.log('getHistory function is running');
      try {
        const response = await fetch('http://localhost:3000/api/weather');
        const history = await response.json();
        console.log('history:', history);
        this.list = history.data
      } catch (error) {
        console.error('An error occurred:', error);
      }
      
    }
  },
  beforeMount() {
   this.getHistory()
}
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style>
