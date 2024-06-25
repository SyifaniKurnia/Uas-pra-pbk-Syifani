<template>
  <q-page padding>
    <div class="background">
      <img src="../assets/sky.jpg" />
    </div>
    <q-card class="text-center q-mx-auto q-mb-md" style="max-width: 300px;">
      <q-card-section>
        <div class="text-h6">Weather Widget</div>
      </q-card-section>
      <q-card-section>
        <q-input
          v-model="location"
          label="Enter location"
          @keyup.enter="fetchWeather"
          outlined
          dense
          class="weather-input"
          :label-color="'white'"
          :input-style="{ color: 'white' }"
        />
      </q-card-section>
      <q-card-section>
        <div class="text-h6">{{ location }}</div>
        <q-img :src="currentWeather.icon" style="width: 100px;" />
        <div>Time: {{ currentWeather.time }}</div>
        <div>Temperature: {{ currentWeather.temperature }}°C</div>
        <div>Weather: {{ currentWeather.weather }}</div>
      </q-card-section>
    </q-card>

    <div v-if="forecastWeather.length" class="forecast-container">
      <q-card class="forecast-card" v-for="(weather, index) in forecastWeather" :key="index">
        <q-card-section>
          <div class="text-h6">{{ location }}</div>
          <q-img :src="weather.icon" style="width: 100px; margin-left: 60px;" />
          <div>Time: {{ weather.time }} </div>
          <div>Temperature: {{ weather.temperature }}°C</div>
          <div>Weather: {{ weather.weather }}</div>
        </q-card-section>
      </q-card>
    </div>
  </q-page>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'

const location = ref('')
const apiKey = 'dd256840354442bed526bb3f2bbf43b7'
const currentWeather = ref(null)
const forecastWeather = ref([])

const fetchWeather = async () => {
  if (!location.value) return

  try {
    const currentResponse = await axios.get(`https://api.openweathermap.org/data/2.5/weather`, {
      params: {
        q: location.value,
        appid: apiKey,
        units: 'metric'
      }
    })
    
    const currentDate = new Date()
    const currentWeatherData = currentResponse.data
    currentWeather.value = {
      time: currentDate.toLocaleTimeString(),
      temperature: currentWeatherData.main.temp,
      weather: currentWeatherData.weather[0].description,
      icon: `http://openweathermap.org/img/wn/${currentWeatherData.weather[0].icon}@2x.png`
    }

    const forecastResponse = await axios.get(`https://api.openweathermap.org/data/2.5/forecast`, {
      params: {
        q: location.value,
        appid: apiKey,
        units: 'metric'
      }
    })
    
    const forecastData = forecastResponse.data.list
    forecastWeather.value = []

    for (let i = 1; i <= 5; i++) {
      const forecast = forecastData[i * 2] 
      const forecastDate = new Date(currentDate.getTime() + i * 2 * 60 * 60 * 1000)
      forecastWeather.value.push({
        time: forecastDate.toLocaleTimeString(),
        temperature: forecast.main.temp,
        weather: forecast.weather[0].description,
        icon: `http://openweathermap.org/img/wn/${forecast.weather[0].icon}@2x.png`
      })
    }
  } catch (error) {
    console.error(error)
  }
}
</script>

<style scoped>
.q-mx-auto {
  margin-left: auto;
  margin-right: auto;
  background-color: transparent;
  backdrop-filter: blur(5px);
  color: aliceblue;
  box-shadow: 0 0 10px gray;
}

.forecast-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 16px;
}

.forecast-card {
  max-width: 250px;
  background-color: transparent;
  backdrop-filter: blur(5px);
  color: aliceblue;
  box-shadow: 0 0 10px gray;
}

.background img {
  position: fixed;
  top: 0;
  left: 0;
  min-height: 90%;
  min-width: 1500px;
  width: 100%;
  height: 100%;
  overflow: hidden;
  z-index: -2;
  object-fit: cover;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
  filter: brightness(0.8);
}

.weather-input .q-field__native {
  color: white !important; /* Set warna teks input putih */
  caret-color: white !important; /* Set warna caret putih */
}

.weather-input::placeholder {
  color: white !important; /* Set warna placeholder putih */
}
</style>
